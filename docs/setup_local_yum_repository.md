    mkdir -p /data/repos/2022_07

## Setup httpd
    yum install httpd
    cp conf/httpd.conf /etc/httpd/conf/httpd.conf
    mkdir /data/repos/images
    chmod -R 777 /data/repos/

##	Repo list
### RHEL 8 Repositories + Ceph

repo id                                                                             repo name
ansible-2.9-for-rhel-8-x86_64-rpms                                                  Red Hat Ansible Engine 2.9 for RHEL 8 x86_64 (RPMs)
rhceph-5-tools-for-rhel-8-x86_64-rpms                                               Red Hat Ceph Storage Tools 5 for RHEL 8 x86_64 (RPMs)
rhel-8-for-x86_64-appstream-rpms                                                    Red Hat Enterprise Linux 8 for x86_64 - AppStream (RPMs)
rhel-8-for-x86_64-baseos-rpms                                                       Red Hat Enterprise Linux 8 for x86_64 - BaseOS (RPMs)

## sync repo data
    yum install -y yum-utils createrepo

    reposync -p /data/repos/2022_07/ansible/2.9/ --download-metadata --repo=ansible-2.9-for-rhel-8-x86_64-rpms
    reposync -p /data/repos/2022_07/ceph/5/ --download-metadata --repo=rhceph-5-tools-for-rhel-8-x86_64-rpms 
    reposync -p /data/repos/2022_07/rhel/8.5/ --download-metadata --repo=rhel-8-for-x86_64-appstream-rpms  
    reposync -p /data/repos/2022_07/rhel/8.5/ --download-metadata --repo=rhel-8-for-x86_64-baseos-rpms

                                                                      

## create repo metadata
    createrepo -v /data/repos/2022_03/centos/8-stream/appstream
    createrepo -v /data/repos/2022_03/centos/8-stream/baseos/
    createrepo -v /data/repos/2022_03/centos/8-stream/extras/ 
    createrepo -v /data/repos/2022_03/centos/8-stream/powertools/ 

    podman login registry.redhat.io --username khanh.chu --password Admin@123
    podman run -d --name localrepos -p 80:8080 -v /data/repos:/var/www:Z registry.redhat.io/rhel8/httpd-24
