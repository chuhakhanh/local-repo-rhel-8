# local-repo-rhel-8
## Introduction
### References
Follow instruction from:
    https://access.redhat.com/documentation/en-us/red_hat_quay/3.7/pdf/deploy_red_hat_quay_for_proof-of-concept_non-production_purposes/red_hat_quay-3.7-deploy_red_hat_quay_for_proof-of-concept_non-production_purposes-en-us.pdf

### Diagram


### Description

Setup 1: 
    Deploy a local repository from RHEL 8

Setup 2:
    Deploy a local container registry - QuayIO


## Prepare

    hostnamectl set-hostname repo-2
    subscription-manager register
    subscription-manager list --available
    subscription-manager subscribe --pool=8a85f9a1790fb0ed017913b420c55b01
    subscription-manager repos --enable=rhceph-5-tools-for-rhel-8-x86_64-rpms
    subscription-manager repos --enable=ansible-2.9-for-rhel-8-x86_64-rpms

### Configure storage
Config /data as Storage location 

    sudo parted -s -a optimal -- /dev/sdb mklabel gpt
    sudo parted -s -a optimal -- /dev/sdb mkpart primary 0% 100%
    sudo parted -s -- /dev/sdb align-check optimal 1
    sudo pvcreate /dev/sdb1
    sudo vgcreate data /dev/sdb1
    sudo lvcreate -n repos -l+100%FREE data
    sudo mkfs.xfs /dev/mapper/data-repos
    mkdir /data
    echo "/dev/mapper/data-repos /data xfs defaults 0 0" >> /etc/fstab
    sudo mount -a

## Setup
### Setup local repository
[Following steps in docs/setup_local_quay.md to setup a local quay](docs/setup_local_quay.md)
### Setup quayio
[Following steps in docs/setup_local_yum_repository.md to setup a local repository](docs/setup_local_yum_repository.md)