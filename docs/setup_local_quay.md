# Deploy local quay.io


## Prepare

### Config database
    yum install -y podman
    podman login registry.redhat.io --username khanh.chu
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

### Create ssl   
Create a Certificate Authority
    cd /root
    openssl genrsa -out rootCA.key 2048
    openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out rootCA.pem
    Country Name (2 letter code) [XX]:VN
    State or Province Name (full name) []:HANOI
    Locality Name (eg, city) [Default City]:HANOI
    Organization Name (eg, company) [Default Company Ltd]:TRAINOCATE
    Organizational Unit Name (eg, section) []:TRAINING
    Common Name (eg, your name or your server's hostname) []:repo-2

Sign a certificate
    openssl genrsa -out ssl.key 2048
    openssl req -new -key ssl.key -out ssl.csr
    Country Name (2 letter code) [XX]:VN
    State or Province Name (full name) []:HANOI
    Locality Name (eg, city) [Default City]:HANOI
    Organization Name (eg, company) [Default Company Ltd]:TRAINOCATE
    Organizational Unit Name (eg, section) []:DOCS
    Common Name (eg, your name or your server's hostname) []:repo-2

    vi /root/openssl.cnf
    [req]
    req_extensions = v3_req
    distinguished_name = req_distinguished_name
    [req_distinguished_name]
    [ v3_req ]
    basicConstraints = CA:FALSE
    keyUsage = nonRepudiation, digitalSignature, keyEncipherment
    subjectAltName = @alt_names
    [alt_names]
    DNS.1 = repo-2
    IP.1 = 10.1.17.35

    openssl x509 -req -in ssl.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out ssl.cert -days 356 -extensions v3_req -extfile openssl.cnf
    cp ssl.cert ssl.crt
We have 3 file: ssl.crt, ssl.cert, ssl.key
### Start quayio in config mode to create config for quay.io
Start quay.io to create bundle configuration
    podman run -d --rm -it --name quay_config -p 8080:8080 -p 443:8443 registry.redhat.io/quay/quay-rhel8:v3.7.3 config secret

Login to http://10.1.17.35:8080 using login: quayconfig/secret and fill the settings

    Server Hostname: repo-2.lab.example.com
    TLS: Red Hat Quay handles TLS
    Certificate: ssl.cert
    Private key: ssl.key

    Database Type: Postgres
    Database Server: 10.1.17.35:5432
    Username: quayuser
    Password: quaypass
    Database Name: quay
    Redis Hostname: 10.1.17.35
    Redis port: 6379 (default)
    Redis password: strongpassword

Download quay-config-ssl.tar.gz and copy to $QUAY/config
### Create POC quay.io

    mkdir $QUAY/config; mkdir $QUAY/storage
    cp ~/Downloads/quay-config.tar.gz $QUAY/config
    cd $QUAY/config
    tar xvf quay-config.tar.gz
    setfacl -m u:1001:-wx $QUAY/storage
    
    podman stop quay_config
    export QUAY=/root/quay
    podman run -d --rm -p 8080:8080 -p 443:8443 \
    --name=quay \
    -v $QUAY/config:/conf/stack:Z \
    -v $QUAY/storage:/datastorage:Z \
    registry.redhat.io/quay/quay-rhel8:v3.7.3

Login to https://repo-2.lab.example.com:8080 to create user: quayadmin/password

## operation 

### Config SSL to trust localCA
    mkdir /etc/containers/certs.d/repo-2.lab.example.com
    cp /root/rootCA.pem /etc/containers/certs.d/repo-2.lab.example.com/ca.crt


### Configure quayio as default insecure local registry 
    cat /etc/containers/registries.conf | grep . | grep -v "#"
    unqualified-search-registries = ["registry.fedoraproject.org", "registry.access.redhat.com", "registry.centos.org", "docker.io"]
    [[registry]]
    location = "repo-2:8080"
    insecure = true
    short-name-mode = "permissive"
    podman system info
    
### Pull and Push required quay image to local quay
Login to registry.redhat.io and pull below images:
    podman login repo-2.lab.example.com --username quayadmin --password password
    podman images
    REPOSITORY                              TAG         IMAGE ID      CREATED      SIZE
    registry.redhat.io/quay/quay-rhel8      v3.7.3      8a5b977d43da  3 weeks ago  887 MB
    registry.redhat.io/rhel8/postgresql-10  1           01dc79535505  4 weeks ago  476 MB
    registry.redhat.io/rhel8/redis-5        1           c9df129d49f8  7 weeks ago  308 MB

### Pull and Push required ceph image to local quay
    registry.redhat.io/openshift4/ose-prometheus                latest      e5edf1ce01c5  4 weeks ago  445 MB
    registry.redhat.io/openshift4/ose-prometheus-node-exporter  latest      74695c709d29  4 weeks ago  319 MB
    registry.redhat.io/openshift4/ose-prometheus-alertmanager   latest      a0913572d333  4 weeks ago  351 MB
    registry.redhat.io/rhceph/rhceph-5-dashboard-rhel8          latest      956620f095a6  4 weeks ago  566 MB
    registry.redhat.io/rhceph/rhceph-5-rhel8                    latest      a4eb511aa22b  4 weeks ago  1.07 GB


    podman tag e5edf1ce01c5 repo-2.lab.example.com/quayadmin/lab/openshift4/ose-prometheus              
    podman tag 74695c709d29 repo-2.lab.example.com/quayadmin/lab/openshift4/ose-prometheus-node-exporter
    podman tag a0913572d333 repo-2.lab.example.com/quayadmin/lab/openshift4/ose-prometheus-alertmanager 
    podman tag 956620f095a6 repo-2.lab.example.com/quayadmin/lab/rhceph/rhceph-5-dashboard-rhel8        
    podman tag a4eb511aa22b repo-2.lab.example.com/quayadmin/lab/rhceph/rhceph-5-rhel8                  
   

    podman push --remove-signatures repo-2.lab.example.com/quayadmin/lab/openshift4/ose-prometheus-node-exporter  
    podman push --remove-signatures repo-2.lab.example.com/quayadmin/lab/openshift4/ose-prometheus-alertmanager   
    podman push --remove-signatures repo-2.lab.example.com/quayadmin/lab/rhceph/rhceph-5-dashboard-rhel8          
    podman push --remove-signatures repo-2.lab.example.com/quayadmin/lab/rhceph/rhceph-5-rhel8                    
    podman push --remove-signatures repo-2.lab.example.com/quayadmin/lab/openshift4/ose-prometheus  




