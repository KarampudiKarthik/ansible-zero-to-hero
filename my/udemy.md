## connect controller to nodes
### create 3 instances, 1 controller and 2 nodes
1. create security group for `controller: control-sg`
2. create security group for `nodes: client-sg`
   
change inbound group roles and  security group roles as shown in fig
>in nodes instances select security group roles> source> control-sg. so these nodes will connect to controller instance

> go to security> add inbound rule to ALL

![image alt](https://github.com/KarampudiKarthik/ansible-zero-to-hero/blob/main/my/images/Capture.PNG?raw=true)

> changes the names of nodes to node 1,2,3 for 3 nodes

## install ansible in controller instance

documentation: https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html#installing-ansible-on-ubuntu
```
sudo apt-get update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

# Inventory and ping module

Establish connection b/w controller and nodes. so, we need inventory file. which consists of host, ip adress, groups. etc

documention: https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html

>create folder, in that folder we need to get all the files like .pem, inventory, playbook
>>we need to change permission for `.pem` file
```
chmod 400 demo_jjj.pem
```
### Create inventory file
g1. for ungrouped
```
all
  hosts:
    web01
      ansible_host:54.242.228.10  # control ec2 ip address
      ansible_user:ec2-user
      ansible_ssh_access_key_file:demo_jjj.pem
```
we need to change some settings
1. switch to root user
```
sudo -i
```
2. go to this folder
```
cd /etc/ansible/
```
3. there we can see ansible.cfg. create another backup file
```
mv ansible.cfg ansible.cfg_backup
```
4. run the command
> it create ansible.cfg file to use to change default nature settings
```
ansible-config init --disabled -t all > ansible.cfg
```
5. open file
```
vim ansible.cfg
```
6. search for host_key_checking. to search in vim editor, use "/" and make it false
   >host_key_checking=False



to check 
```
ansible web01 -m ping -i inventory
```
2.  for grouped
```
all
  hosts:
    web01
      ansible_host:54.242.228.10  # control ec2 ip address
    web01
      ansible_host:54.242.228.10
    web01
      ansible_host:54.242.228.10


  children:
    webservers:
      hosts:
        web01:
        web02:
    dbservers:
      hosts:
        db01:
      vars:
        ansible_user:ec2-user
        ansible_ssh_access_key_file:demo_jjj.pem
        
```
3. for group in a grouped
```
all
  hosts:
    web01
      ansible_host:54.242.228.10  # control ec2 ip address
    web01
      ansible_host:54.242.228.10
    web01
      ansible_host:54.242.228.10


  children:
    webservers:
      hosts:
        web01:
        web02:
    dbservers:
      hosts:
        db01:
    groups_3:
      children:
        webservers:
        dbservers:
      vars:
        ansible_user:ec2-user
        ansible_ssh_access_key_file:demo_jjj.pem
        
```

## Ad Hoc command

documentation: https://docs.ansible.com/ansible/latest/command_guide/intro_adhoc.html

ad hoc commands are great for tasks you repeat rarely. For example, if you want to power off all the machines in your lab for Christmas vacation, you could execute a quick one-liner in Ansible without writing a playbook. 

Example: Ensure a service is started on all webservers:
```
$ ansible webservers -m ansible.builtin.service -a "name=httpd state=started"
```

## Playbook & Modules

playbook documentation: https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_intro.html
modules documentation: https://docs.ansible.com/ansible/latest/collections/index_module.html

```
---
- name: Update web servers
  hosts: webservers

  tasks:
  - name: Ensure apache is at the latest version
    ansible.builtin.yum:
      name: httpd
      state: latest

  - name: Write the apache config file
    ansible.builtin.template:
      src: /srv/httpd.j2
      dest: /etc/httpd.conf

- name: Update db servers
  hosts: databases

  tasks:
  - name: Ensure postgresql is at the latest version
    ansible.builtin.yum:
      name: postgresql
      state: latest

  - name: Ensure that postgresql is started
    ansible.builtin.service:
      name: postgresql
      state: started
```

to check
```
ansible-playbook -i inventory playbook.yml
```
