# ansible_demo
ansible demo

# Note for ansible command

###### Check playbook syntax
>ansible-playbook --syntax-check copy_hosts.yml

###### Check playbook environment (dry run)
>ansible-playblook -C copy_hosts.yml

###### Check [module] docs
>ansible-doc [module]

#### Ansible-playbook for more information
>-v: show tasks
>-vv: show settings and results
>-vvv: includeing netowrk information
>-vvvv: More plugin information

## Ansible-playbook variables
###### Kinds of ansible variables
* user name
* install/remove app
* system start/stop app
* Build/remove filename
* download from web

###### variables
* global scope: ansible.cfg
* play scope: in the playbook
* host scope: in the inventory

###### priority
global->playbook->host

###### group_vars/ and host_vars/
group_vars/ instance-group name: like webiste, devel, mylab ...
hosts_vars/ instance name: like dbserver1, webserver1...

###### ansible directory hierarchy
ansible-init
↳-- ansible.cfg
↳-- group_vars
⇃   ↳-- devel
⇃   ↳-- mylab
⇃   ↳-- mwebsite
↳-- host_vars
⇃   ↳-- dbserver1



## Ansible-vault encrypts variables and files
###### basic syntax
vim ~/.bashrc
export EDITOR=vim

source ~/.bashrc

ansible-vault create file.yml
ansible-vault view file.yml
ansible-vault edit file.yml

###### password file
vim mypassword.txt

chmod 600 mypassword.txt
ansible-vault edit --vault-password-file=mypassword.txt secret1.yml
###### encrypt yml-file
ansible-vault encrypt --vault-password-file=mypassword.txt add_service.yml --output=secret2.yml

###### decrypt yml-file
ansible-vault decrypt --vault-password-file=mypassword.txt secret1.yml

###### rekey : renew password
ansible-vault encrypt secret1.yml
ansible-vault rekey secret1.yml

## Ansible facts find out managed hosts
###### setup module
ansible webserver1 -m setup

gather_facts: no  disable setup module

###### debug module
show ansible_facts variables
---
  - name: list my managed hosts facts
    hosts: website
    
    tasks:
      - name: list managed hosts facts
        debug:
          var: ansible_facts
          
ansible_facts['hostname']
ansible_facts['fqdn']
ansible_facts['defalut_ipv4']['address']
ansible_facts['interfaces']
ansible_facts['devices']['vda']['partitions']['vda1']['size']
ansible_facts['dns']['nameservers']
ansible_facts['kernel']

###### user defined facts
###### /etc/ansible/facts.d/*.fact
sudo -i
mkdir /etc/ansible/facts.d
setfac1 -m u:user:rwx /etc/ansible/facts.d

vim /etc/ansible/facts.d/myset.fact
