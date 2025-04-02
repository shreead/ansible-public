# Ansible Roles

Collection of ansible roles to deploy docker containers. 

##  Variables
Global/Per-Machine variables:
```
DOCKER_PATH: '/path/to/docker/bind/mount'
MY_DOMAIN: 'mydomain.tld'
IP_ADDRESS: 'IP.OF.LOCAL.MACHINE'
USER_NAME: 'linux-username'
MY_EMAIL: 'linux-user@email'
```

Role variables:
Will be listed at the top of the yml file


## Roles
Provisioning:
- timezone
- apt-update-upgrade
- apt-install
- unattended-upgrades
- motd-clean
- motd-hide-esm
- dotfiles
- ssh-keys-github

Docker:
- docker-install
- dockershare-mount-nfs
- dockershare-backuptask
