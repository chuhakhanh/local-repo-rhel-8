## Setup httpd
    mkdir -p /data/repos/html
    chmod -R 777 /data/repos/
    podman login registry.redhat.io --username khanh.chu
    podman run -d --name localrepos -p 80:8080 -v /data/repos:/var/www:Z registry.redhat.io/rhel8/httpd-24

##	Sync Repositories
### RHEL 8 Repositories + Ceph

    repo id                                                                             repo name
    ansible-2.9-for-rhel-8-x86_64-rpms                                                  Red Hat Ansible Engine 2.9 for RHEL 8 x86_64 (RPMs)
    rhceph-5-tools-for-rhel-8-x86_64-rpms                                               Red Hat Ceph Storage Tools 5 for RHEL 8 x86_64 (RPMs)
    rhel-8-for-x86_64-appstream-rpms                                                    Red Hat Enterprise Linux 8 for x86_64 - AppStream (RPMs)
    rhel-8-for-x86_64-baseos-rpms                                                       Red Hat Enterprise Linux 8 for x86_64 - BaseOS (RPMs)

### sync repo data

    yum install -y yum-utils createrepo
    mkdir -p /data/repos/2022_07
    reposync -p /data/repos/html/2022_07/ansible/2.9/ --download-metadata --repo=ansible-2.9-for-rhel-8-x86_64-rpms
    reposync -p /data/repos/html/2022_07/ceph/5/ --download-metadata --repo=rhceph-5-tools-for-rhel-8-x86_64-rpms 
    reposync -p /data/repos/html/2022_07/rhel/8.5/appstream --download-metadata --repo=rhel-8-for-x86_64-appstream-rpms  
    reposync -p /data/repos/html/2022_07/rhel/8.5/baseos --download-metadata --repo=rhel-8-for-x86_64-baseos-rpms

                                                                      

### create repo metadata
    createrepo -v /data/repos/html/2022_07/ansible/2.9/
    createrepo -v /data/repos/html/2022_07/ceph/5/
    createrepo -v /data/repos/html/2022_07/rhel/8.5/appstream 
    createrepo -v /data/repos/html/2022_07/rhel/8.5/baseos 



## Setup repo in client
    echo "10.1.17.35 repo-2" >> /etc/hosts
    cp local-repo-rhel-8/config/2022_07.repo /etc/yum.repos.d/