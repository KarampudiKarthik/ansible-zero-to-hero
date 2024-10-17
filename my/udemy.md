# connect controller to nodes
## create 3 instances, 1 controller and 2 nodes
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

```
all
  hosts:
    web01
      ansible_host:54.242.228.10  # control ec2 ip address
      ansible_user:ec2-user
      ansible_ssh_access_key_file:demo_jjj.pem
```
to check 
```
ansible web01 -m ping -i inventory
```








