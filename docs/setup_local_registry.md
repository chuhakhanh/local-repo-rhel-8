# Deploy registry

## Configure storage
sudo dnf install python3-pip
sudo dnf install python3-devel libffi-devel gcc openssl-devel python3-libselinux
pip3 install 'ansible==2.10.7'
pip3 install git+https://opendev.org/openstack/kolla-ansible@stable/xena
cp -r  /usr/local/share/kolla-ansible/etc_examples/kolla/ /etc/

docker run -d -p 4000:5000 --restart=always --name registry -v /data/registry:/var/lib/registry registry:2

## List of required images
quay.io/openstack.kolla/centos-source-nova-compute                    xena      9e8825ca4e0d   4 days ago   2.43GB
quay.io/openstack.kolla/centos-source-neutron-server                  xena      b4b36ac683f0   4 days ago   1.67GB
quay.io/openstack.kolla/centos-source-neutron-l3-agent                xena      21e3d0774c93   4 days ago   1.72GB
quay.io/openstack.kolla/centos-source-cinder-volume                   xena      dd9c9c0f006d   4 days ago   2.47GB
quay.io/openstack.kolla/centos-source-neutron-dhcp-agent              xena      e7f5fc1aeb12   4 days ago   1.67GB
quay.io/openstack.kolla/centos-source-neutron-openvswitch-agent       xena      d55b59bfb065   4 days ago   1.67GB
quay.io/openstack.kolla/centos-source-neutron-metadata-agent          xena      d259d5d046eb   4 days ago   1.67GB
quay.io/openstack.kolla/centos-source-cloudkitty-api                  xena      69bafe9823af   4 days ago   1.4GB
quay.io/openstack.kolla/centos-source-cloudkitty-processor            xena      cbe574f7ec43   4 days ago   1.4GB
quay.io/openstack.kolla/centos-source-nova-ssh                        xena      9fc129f7f0f6   4 days ago   1.75GB
quay.io/openstack.kolla/centos-source-nova-novncproxy                 xena      dd38b5b56c81   4 days ago   1.8GB
quay.io/openstack.kolla/centos-source-nova-scheduler                  xena      a14d91010ce5   4 days ago   1.71GB
quay.io/openstack.kolla/centos-source-nova-conductor                  xena      b83950a8f20e   4 days ago   1.71GB
quay.io/openstack.kolla/centos-source-nova-api                        xena      78e91d86434c   4 days ago   1.71GB
quay.io/openstack.kolla/centos-source-cinder-scheduler                xena      4b5af75a0892   4 days ago   1.84GB
quay.io/openstack.kolla/centos-source-cinder-api                      xena      3ec97b5acc41   4 days ago   1.84GB
quay.io/openstack.kolla/centos-source-placement-api                   xena      ccfaef9475d4   4 days ago   1.47GB
quay.io/openstack.kolla/centos-source-glance-api                      xena      9d54c6a10ef6   4 days ago   1.59GB
quay.io/openstack.kolla/centos-source-keystone-ssh                    xena      cd1955ce2d61   4 days ago   1.61GB
quay.io/openstack.kolla/centos-source-heat-api                        xena      bde3963641e9   4 days ago   1.45GB
quay.io/openstack.kolla/centos-source-heat-engine                     xena      f4b63c4c9d3f   4 days ago   1.45GB
quay.io/openstack.kolla/centos-source-keystone-fernet                 xena      922b86727cae   4 days ago   1.61GB
quay.io/openstack.kolla/centos-source-heat-api-cfn                    xena      65ad1c3e399c   4 days ago   1.45GB
quay.io/openstack.kolla/centos-source-keystone                        xena      aae38d78585b   4 days ago   1.61GB
quay.io/openstack.kolla/centos-source-horizon                         xena      c4bcfb862854   4 days ago   1.61GB
quay.io/openstack.kolla/centos-source-nova-libvirt                    xena      172aca025b0c   4 days ago   2.24GB
quay.io/openstack.kolla/centos-source-prometheus-blackbox-exporter    xena      41a9606cdfe6   4 days ago   867MB
quay.io/openstack.kolla/centos-source-openvswitch-vswitchd            xena      c308fac7db7a   4 days ago   980MB
quay.io/openstack.kolla/centos-source-openvswitch-db-server           xena      e4a917fe8aa3   4 days ago   980MB
quay.io/openstack.kolla/centos-source-prometheus-memcached-exporter   xena      3daadbacc157   4 days ago   840MB
quay.io/openstack.kolla/centos-source-prometheus-mysqld-exporter      xena      1b5d9c2c2b30   4 days ago   841MB
quay.io/openstack.kolla/centos-source-prometheus-v2-server            xena      f04196c6b346   4 days ago   993MB
quay.io/openstack.kolla/centos-source-prometheus-alertmanager         xena      1653c1341f6c   4 days ago   876MB
quay.io/openstack.kolla/centos-source-prometheus-node-exporter        xena      8a9ad570eb08   4 days ago   844MB
quay.io/openstack.kolla/centos-source-prometheus-cadvisor             xena      d3a1173d441e   4 days ago   866MB
quay.io/openstack.kolla/centos-source-prometheus-openstack-exporter   xena      1d5e86847a62   4 days ago   842MB
quay.io/openstack.kolla/centos-source-kolla-toolbox                   xena      172f271a37dd   4 days ago   1.41GB
quay.io/openstack.kolla/centos-source-mariadb-server                  xena      6dafa02bfe81   4 days ago   1.34GB
quay.io/openstack.kolla/centos-source-iscsid                          xena      2a848b8b7e72   4 days ago   858MB
quay.io/openstack.kolla/centos-source-influxdb                        xena      312b9b506b54   4 days ago   1.01GB
quay.io/openstack.kolla/centos-source-rabbitmq                        xena      7d4d30a6dc25   4 days ago   910MB
quay.io/openstack.kolla/centos-source-memcached                       xena      80a4c8df3437   4 days ago   888MB
quay.io/openstack.kolla/centos-source-cron                            xena      032bdf9d92e6   4 days ago   853MB

## pull 43 images

docker pull quay.io/openstack.kolla/centos-source-nova-compute:xena
docker pull quay.io/openstack.kolla/centos-source-neutron-server:xena
docker pull quay.io/openstack.kolla/centos-source-neutron-l3-agent:xena
docker pull quay.io/openstack.kolla/centos-source-cinder-volume:xena
docker pull quay.io/openstack.kolla/centos-source-neutron-dhcp-agent:xena
docker pull quay.io/openstack.kolla/centos-source-neutron-openvswitch-agent:xena
docker pull quay.io/openstack.kolla/centos-source-neutron-metadata-agent:xena
docker pull quay.io/openstack.kolla/centos-source-cloudkitty-api:xena
docker pull quay.io/openstack.kolla/centos-source-cloudkitty-processor:xena
docker pull quay.io/openstack.kolla/centos-source-nova-ssh:xena
docker pull quay.io/openstack.kolla/centos-source-nova-novncproxy:xena
docker pull quay.io/openstack.kolla/centos-source-nova-scheduler:xena
docker pull quay.io/openstack.kolla/centos-source-nova-conductor:xena
docker pull quay.io/openstack.kolla/centos-source-nova-api:xena                        
docker pull quay.io/openstack.kolla/centos-source-cinder-scheduler:xena                
docker pull quay.io/openstack.kolla/centos-source-cinder-api:xena                      
docker pull quay.io/openstack.kolla/centos-source-placement-api:xena                   
docker pull quay.io/openstack.kolla/centos-source-glance-api:xena                      
docker pull quay.io/openstack.kolla/centos-source-keystone-ssh:xena                    
docker pull quay.io/openstack.kolla/centos-source-heat-api:xena                        
docker pull quay.io/openstack.kolla/centos-source-heat-engine:xena                     
docker pull quay.io/openstack.kolla/centos-source-keystone-fernet:xena                 
docker pull quay.io/openstack.kolla/centos-source-heat-api-cfn:xena                    
docker pull quay.io/openstack.kolla/centos-source-keystone:xena                        
docker pull quay.io/openstack.kolla/centos-source-horizon:xena                         
docker pull quay.io/openstack.kolla/centos-source-nova-libvirt:xena                    
docker pull quay.io/openstack.kolla/centos-source-prometheus-blackbox-exporter:xena    
docker pull quay.io/openstack.kolla/centos-source-openvswitch-vswitchd:xena            
docker pull quay.io/openstack.kolla/centos-source-openvswitch-db-server:xena           
docker pull quay.io/openstack.kolla/centos-source-prometheus-memcached-exporter:xena   
docker pull quay.io/openstack.kolla/centos-source-prometheus-mysqld-exporter:xena      
docker pull quay.io/openstack.kolla/centos-source-prometheus-v2-server:xena            
docker pull quay.io/openstack.kolla/centos-source-prometheus-alertmanager:xena         
docker pull quay.io/openstack.kolla/centos-source-prometheus-node-exporter:xena        
docker pull quay.io/openstack.kolla/centos-source-prometheus-cadvisor:xena             
docker pull quay.io/openstack.kolla/centos-source-prometheus-openstack-exporter:xena   
docker pull quay.io/openstack.kolla/centos-source-kolla-toolbox:xena                   
docker pull quay.io/openstack.kolla/centos-source-mariadb-server:xena                  
docker pull quay.io/openstack.kolla/centos-source-iscsid:xena                          
docker pull quay.io/openstack.kolla/centos-source-influxdb:xena                        
docker pull quay.io/openstack.kolla/centos-source-rabbitmq:xena                        
docker pull quay.io/openstack.kolla/centos-source-memcached:xena                       
docker pull quay.io/openstack.kolla/centos-source-cron:xena                            


## tag 43 images


docker tag quay.io/openstack.kolla/centos-source-nova-compute:xena repo-1:4000/openstack.kolla/centos-source-nova-compute:xena
docker tag quay.io/openstack.kolla/centos-source-neutron-server:xena repo-1:4000/openstack.kolla/centos-source-neutron-server:xena
docker tag quay.io/openstack.kolla/centos-source-neutron-l3-agent:xena repo-1:4000/openstack.kolla/centos-source-neutron-l3-agent:xena
docker tag quay.io/openstack.kolla/centos-source-cinder-volume:xena repo-1:4000/openstack.kolla/centos-source-cinder-volume:xena
docker tag quay.io/openstack.kolla/centos-source-neutron-dhcp-agent:xena repo-1:4000/openstack.kolla/centos-source-neutron-dhcp-agent:xena
docker tag quay.io/openstack.kolla/centos-source-neutron-openvswitch-agent:xena repo-1:4000/openstack.kolla/centos-source-neutron-openvswitch-agent:xena
docker tag quay.io/openstack.kolla/centos-source-neutron-metadata-agent:xena repo-1:4000/openstack.kolla/centos-source-neutron-metadata-agent:xena
docker tag quay.io/openstack.kolla/centos-source-cloudkitty-api:xena repo-1:4000/openstack.kolla/centos-source-cloudkitty-api:xena
docker tag quay.io/openstack.kolla/centos-source-cloudkitty-processor:xena repo-1:4000/openstack.kolla/centos-source-cloudkitty-processor:xena
docker tag quay.io/openstack.kolla/centos-source-nova-ssh:xena repo-1:4000/openstack.kolla/centos-source-nova-ssh:xena
docker tag quay.io/openstack.kolla/centos-source-nova-novncproxy:xena repo-1:4000/openstack.kolla/centos-source-nova-novncproxy:xena
docker tag quay.io/openstack.kolla/centos-source-nova-scheduler:xena repo-1:4000/openstack.kolla/centos-source-nova-scheduler:xena
docker tag quay.io/openstack.kolla/centos-source-nova-conductor:xena repo-1:4000/openstack.kolla/centos-source-nova-conductor:xena
docker tag quay.io/openstack.kolla/centos-source-nova-api:xena repo-1:4000/openstack.kolla/centos-source-nova-api:xena
docker tag quay.io/openstack.kolla/centos-source-cinder-scheduler:xena repo-1:4000/openstack.kolla/centos-source-cinder-scheduler:xena
docker tag quay.io/openstack.kolla/centos-source-cinder-api:xena repo-1:4000/openstack.kolla/centos-source-cinder-api:xena
docker tag quay.io/openstack.kolla/centos-source-placement-api:xena repo-1:4000/openstack.kolla/centos-source-placement-api:xena
docker tag quay.io/openstack.kolla/centos-source-glance-api:xena repo-1:4000/openstack.kolla/centos-source-glance-api:xena
docker tag quay.io/openstack.kolla/centos-source-keystone-ssh:xena repo-1:4000/openstack.kolla/centos-source-keystone-ssh:xena
docker tag quay.io/openstack.kolla/centos-source-heat-api:xena repo-1:4000/openstack.kolla/centos-source-heat-api:xena
docker tag quay.io/openstack.kolla/centos-source-heat-engine:xena repo-1:4000/openstack.kolla/centos-source-heat-engine:xena
docker tag quay.io/openstack.kolla/centos-source-keystone-fernet:xena repo-1:4000/openstack.kolla/centos-source-keystone-fernet:xena
docker tag quay.io/openstack.kolla/centos-source-heat-api-cfn:xena repo-1:4000/openstack.kolla/centos-source-heat-api-cfn:xena
docker tag quay.io/openstack.kolla/centos-source-keystone:xena repo-1:4000/openstack.kolla/centos-source-keystone:xena
docker tag quay.io/openstack.kolla/centos-source-horizon:xena repo-1:4000/openstack.kolla/centos-source-horizon:xena
docker tag quay.io/openstack.kolla/centos-source-nova-libvirt:xena repo-1:4000/openstack.kolla/centos-source-nova-libvirt:xena
docker tag quay.io/openstack.kolla/centos-source-prometheus-blackbox-exporter:xena repo-1:4000/openstack.kolla/centos-source-prometheus-blackbox-exporter:xena
docker tag quay.io/openstack.kolla/centos-source-openvswitch-vswitchd:xena repo-1:4000/openstack.kolla/centos-source-openvswitch-vswitchd:xena
docker tag quay.io/openstack.kolla/centos-source-openvswitch-db-server:xena repo-1:4000/openstack.kolla/centos-source-openvswitch-db-server:xena
docker tag quay.io/openstack.kolla/centos-source-prometheus-memcached-exporter:xena repo-1:4000/openstack.kolla/centos-source-prometheus-memcached-exporter:xena
docker tag quay.io/openstack.kolla/centos-source-prometheus-mysqld-exporter:xena repo-1:4000/openstack.kolla/centos-source-prometheus-mysqld-exporter:xena
docker tag quay.io/openstack.kolla/centos-source-prometheus-v2-server:xena repo-1:4000/openstack.kolla/centos-source-prometheus-v2-server:xena
docker tag quay.io/openstack.kolla/centos-source-prometheus-alertmanager:xena repo-1:4000/openstack.kolla/centos-source-prometheus-alertmanager:xena
docker tag quay.io/openstack.kolla/centos-source-prometheus-node-exporter:xena repo-1:4000/openstack.kolla/centos-source-prometheus-node-exporter:xena
docker tag quay.io/openstack.kolla/centos-source-prometheus-cadvisor:xena repo-1:4000/openstack.kolla/centos-source-prometheus-cadvisor:xena
docker tag quay.io/openstack.kolla/centos-source-prometheus-openstack-exporter:xena repo-1:4000/openstack.kolla/centos-source-prometheus-openstack-exporter:xena
docker tag quay.io/openstack.kolla/centos-source-kolla-toolbox:xena repo-1:4000/openstack.kolla/centos-source-kolla-toolbox:xena
docker tag quay.io/openstack.kolla/centos-source-mariadb-server:xena     repo-1:4000/openstack.kolla/centos-source-mariadb-server:xena              
docker tag quay.io/openstack.kolla/centos-source-iscsid:xena             repo-1:4000/openstack.kolla/centos-source-iscsid:xena                      
docker tag quay.io/openstack.kolla/centos-source-influxdb:xena           repo-1:4000/openstack.kolla/centos-source-influxdb:xena                    
docker tag quay.io/openstack.kolla/centos-source-rabbitmq:xena           repo-1:4000/openstack.kolla/centos-source-rabbitmq:xena                    
docker tag quay.io/openstack.kolla/centos-source-memcached:xena          repo-1:4000/openstack.kolla/centos-source-memcached:xena                   
docker tag quay.io/openstack.kolla/centos-source-cron:xena               repo-1:4000/openstack.kolla/centos-source-cron:xena 

## push 43 images

docker push	repo-1:4000/openstack.kolla/centos-source-cinder-api:xena
docker push	repo-1:4000/openstack.kolla/centos-source-cinder-scheduler:xena
docker push	repo-1:4000/openstack.kolla/centos-source-cinder-volume:xena
docker push	repo-1:4000/openstack.kolla/centos-source-cloudkitty-api:xena
docker push	repo-1:4000/openstack.kolla/centos-source-cloudkitty-processor:xena
docker push	repo-1:4000/openstack.kolla/centos-source-cron:xena
docker push	repo-1:4000/openstack.kolla/centos-source-glance-api:xena
docker push	repo-1:4000/openstack.kolla/centos-source-heat-api:xena
docker push	repo-1:4000/openstack.kolla/centos-source-heat-api-cfn:xena
docker push	repo-1:4000/openstack.kolla/centos-source-heat-engine:xena
docker push	repo-1:4000/openstack.kolla/centos-source-horizon:xena
docker push	repo-1:4000/openstack.kolla/centos-source-influxdb:xena
docker push	repo-1:4000/openstack.kolla/centos-source-iscsid:xena
docker push	repo-1:4000/openstack.kolla/centos-source-keystone:xena
docker push	repo-1:4000/openstack.kolla/centos-source-keystone-fernet:xena
docker push	repo-1:4000/openstack.kolla/centos-source-keystone-ssh:xena
docker push	repo-1:4000/openstack.kolla/centos-source-kolla-toolbox:xena
docker push	repo-1:4000/openstack.kolla/centos-source-mariadb-server:xena
docker push	repo-1:4000/openstack.kolla/centos-source-memcached:xena
docker push	repo-1:4000/openstack.kolla/centos-source-neutron-dhcp-agent:xena
docker push	repo-1:4000/openstack.kolla/centos-source-neutron-l3-agent:xena
docker push	repo-1:4000/openstack.kolla/centos-source-neutron-metadata-agent:xena
docker push	repo-1:4000/openstack.kolla/centos-source-neutron-openvswitch-agent:xena
docker push	repo-1:4000/openstack.kolla/centos-source-neutron-server:xena
docker push	repo-1:4000/openstack.kolla/centos-source-nova-api:xena
docker push	repo-1:4000/openstack.kolla/centos-source-nova-compute:xena
docker push	repo-1:4000/openstack.kolla/centos-source-nova-conductor:xena
docker push	repo-1:4000/openstack.kolla/centos-source-nova-libvirt:xena
docker push	repo-1:4000/openstack.kolla/centos-source-nova-novncproxy:xena
docker push	repo-1:4000/openstack.kolla/centos-source-nova-scheduler:xena
docker push	repo-1:4000/openstack.kolla/centos-source-nova-ssh:xena
docker push	repo-1:4000/openstack.kolla/centos-source-openvswitch-db-server:xena
docker push	repo-1:4000/openstack.kolla/centos-source-openvswitch-vswitchd:xena
docker push	repo-1:4000/openstack.kolla/centos-source-placement-api:xena
docker push	repo-1:4000/openstack.kolla/centos-source-prometheus-alertmanager:xena
docker push	repo-1:4000/openstack.kolla/centos-source-prometheus-blackbox-exporter:xena
docker push	repo-1:4000/openstack.kolla/centos-source-prometheus-cadvisor:xena
docker push	repo-1:4000/openstack.kolla/centos-source-prometheus-memcached-exporter:xena
docker push	repo-1:4000/openstack.kolla/centos-source-prometheus-mysqld-exporter:xena
docker push	repo-1:4000/openstack.kolla/centos-source-prometheus-node-exporter:xena
docker push	repo-1:4000/openstack.kolla/centos-source-prometheus-openstack-exporter:xena
docker push	repo-1:4000/openstack.kolla/centos-source-prometheus-v2-server:xena
docker push	repo-1:4000/openstack.kolla/centos-source-rabbitmq:xena


## 

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