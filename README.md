# Ansible Basics

## Install

```
# install
pipx install --include-deps ansible

# upgrade
pipx upgrade --include-injected ansible
```

In arch: https://wiki.archlinux.org/title/Ansible

```
sudo pacman -Syu ansible
```
Generate blank config:
```
ansible-config init --disabled > ansible.cfg
```


Inventory file (can be in the local directory): more details below
```
$ nano hosts
[vms]
server01 ansible-host=10.10.0.3 ansible_port=22 ansible_user=some-user
server02 ansible_host=10.10.0.4 ansible_port=22 ansible_user=some-user
```

## Ad-hoc commands

```
ansible all --key-file ~/.ssh/ansible-key -i inventory -m ping --ask-become-pass

ansible all -i inventory -m gather_facts

ansible all -i inventory -m gather_facts --limit server01 | grep ansible_distribution
```

## Elevated ad-hoc commands

```
ansible all -i inventory -m apt -a update_cache=yes --become --ask-become-pass

upgrade=yes/dist 
name=sl
"name=sl state=latest"
```


## Ansible Playbook

### Template

```
---
- hosts: all
  become: true
  tasks:
    - name: ...
      ...
```

### Sample
```
---

- hosts: all
  become: true

  tasks:
  - name: update apt repository index
    apt:
      update_cache: yes
    when: ansible_distribution in ["Debian", "Ubuntu"]

  - name: install sl package
    apt:
      name: sl
      state: latest
    when: ansible_distribution in ["Debian", "Ubuntu"]
```

To run the playbook:
```
ansible-playbook --ask-become-pass -i inventory install_sl.yml 
```


### Tags

```
---

- hosts: all
  become: true
  tasks:

  - name: update packages
    tags: always
    apt:
      update_cache: yes
    when: ansible_distribution in ["Debian", "Ubuntu"]

  - name: update and install stress and s-tui
    tags: ubuntu,debian,stess,s-tui
    apt:
      name:
        - stress
        - s-tui
      state: latest
    when: ansible_distribution in ["Debian", "Ubuntu"]

```


```
ansible-playbook --tags "htop,powertop" -i inventory install.yml --ask-become-pass
```

## Inventory
Define host name ip address etc

nano inventory
```
[all:vars]
ansible_user='some-user'
ansible_become=yes
ansible_become_method=sudo
ansible_python_interpreter='/usr/bin/env python3'

[ubuntutest]
ubuntu-test

[dockertest]
docker-test ansible_host=10.10.xx.xx ansible_port=xxxx ansible_user=some-user
```

List inventory
```
ansible-inventory -i hosts --list
```

ubuntu-test defined in .ssh/config

### Usage
```
ansible all -i hosts -m ping
```
