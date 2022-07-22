# Deploy local quay.io


## Prepare

### Config database
    yum install -y podman
    podman login registry.redhat.io --username khanh.chu --password Admin@123
    export QUAY=/root/quay
    mkdir -p $QUAY/postgres-quay
    setfacl -m u:26:-wx $QUAY/postgres-quay
    podman run -d --rm --name postgresql-quay \
    -e POSTGRESQL_USER=quayuser \
    -e POSTGRESQL_PASSWORD=quaypass \
    -e POSTGRESQL_DATABASE=quay \
    -e POSTGRESQL_ADMIN_PASSWORD=adminpass \
    -p 5432:5432 \
    -v $QUAY/postgres-quay:/var/lib/pgsql/data:Z \
    registry.redhat.io/rhel8/postgresql-10:1  
    podman exec -it postgresql-quay /bin/bash -c 'echo "CREATE EXTENSION IF NOT EXISTS pg_trgm" | psql -d quay -U postgres'

### Config redis
    podman run -d --rm --name redis \
    -p 6379:6379 \
    -e REDIS_PASSWORD=strongpassword \
    registry.redhat.io/rhel8/redis-5:1    

## Setup

### Start quay.io to create bundle configuration
    podman run -d --rm -it --name quay_config -p 80:8080 -p 443:8443 registry.redhat.io/quay/quay-rhel8:v3.7.3 config secret
Login to http://10.1.17.35 using login: quayconfig/secret

### Create POC quay.io
    mkdir $QUAY/config; mkdir $QUAY/storage
    cp ~/Downloads/quay-config.tar.gz $QUAY/config
    cd $QUAY/config
    tar xvf quay-config.tar.gz
    setfacl -m u:1001:-wx $QUAY/storage
    
    podman run -d --rm -p 80:8080 -p 443:8443 \
    --name=quay \
    -v $QUAY/config:/conf/stack:Z \
    -v $QUAY/storage:/datastorage:Z \
    registry.redhat.io/quay/quay-rhel8:v3.7.3

Login to http://10.1.17.35 using login: quayadmin/password
## Use

### Push image to server
    podman login --tls-verify=false 10.1.17.35 --username quayadmin --password password
    podman images
    REPOSITORY                              TAG         IMAGE ID      CREATED      SIZE
    registry.redhat.io/quay/quay-rhel8      v3.7.3      8a5b977d43da  3 weeks ago  887 MB
    registry.redhat.io/rhel8/postgresql-10  1           01dc79535505  4 weeks ago  476 MB
    registry.redhat.io/rhel8/redis-5        1           c9df129d49f8  7 weeks ago  308 MB

    podman tag 8a5b977d43da 10.1.17.35/quayadmin/lab/quay/quay-rhel8
    podman tag 01dc79535505 10.1.17.35/quayadmin/lab/rhel8/postgresql-10
    podman tag c9df129d49f8 10.1.17.35/quayadmin/lab/rhel8/redis-5

    podman tag 8a5b977d43da 10.1.17.35/quayadmin/lab

    podman push --remove-signatures --tls-verify=false 10.1.17.35/quayadmin/lab/quay/quay-rhel8 
    podman push --remove-signatures --tls-verify=false 10.1.17.35/quayadmin/lab/rhel8/postgresql-10
    podman push --remove-signatures --tls-verify=false 10.1.17.35/quayadmin/lab/rhel8/redis-5

### Pull image to client
    podman login --tls-verify=false 10.1.17.35 --username quayadmin --password password
    podman pull --tls-verify=false 10.1.17.35/quayadmin/lab/quay/quay-rhel8
### remove image
    podman rmi <image id>

mv /etc/systemd/system/docker.service /etc/systemd/system/docker.service_save
vi /etc/sysconfig/docker
vi /etc/docker/daemon.json
{
  "insecure-registries" : ["repo-1:4000"]
}

{
    "bridge": "none",
    "insecure-registries" : ["repo-1:4000"],
    "ip-forward": false,
    "iptables": false,
    "log-opts": {
        "max-file": "5",
        "max-size": "50m"
    }
}
~
## On new client

curl -X GET http://repo-1:4000/v2/_catalog | jq -r 
docker pull repo-1:4000/openstack.kolla/centos-source-mariadb-server:xena

curl -sSL https://get.docker.io | bash
echo 'INSECURE_REGISTRY="--insecure-registry repo-1:4000"' > /etc/sysconfig/docker

tee /etc/systemd/system/docker.service <<-'EOF'
# CentOS
[Service]
MountFlags=shared
EnvironmentFile=/etc/sysconfig/docker
ExecStart=/usr/bin/docker daemon $INSECURE_REGISTRY
EOF

systemctl daemon-reload
systemctl restart docker


docker pull repo-1:4000/centos-source-cinder-api:xena


## Create deployment docker for Xena
docker build -t kolla-deploy:xena --force-rm -f kolla-deploy.dockerfile .
docker run -d --name deploy-1 kolla-deploy:xena 