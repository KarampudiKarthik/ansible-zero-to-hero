## Ansible configuration

documentation: https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings

By default ansible is configured, but we can also change default configuration
### order of ansible config
1. ansible_config(environmental variable if set)
2. ansible.cfg(in current directory)
3. ~/.ansible.cfg(in home directory)  # hidden file
4. /etc/ansible/ansible.cfg

in /etc/ansible/ansible.cfg there are more than 1000 lines and there are multiple sections like defaults, powershell, variables etc

### example ansible.cfg
```
[defaults]
host_key_checking=False
forks=5
inventory=./inventory.yml

[previlage_escalation]
become=True
become_method=sudo
```

## Variables
documentation: https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html

we can also create another file for variable. it will connect with playbook
> playbook variables are priority than variable file

##files modules
documentation: https://docs.ansible.com/ansible/2.8/modules/list_of_files_modules.html

## Roles
we can see in a tree structure if we already in a project director
```
sudo apt install tree -y
```

```
tree dir_name
```
to create from scratch
1. create a folder name role
2. cd role
3. run command
```
ansible-galaxy init post-install
```
it will install tree structure. to check
```
tree
```
![image_alt](https://github.com/KarampudiKarthik/ansible-zero-to-hero/blob/main/my/images/Capture1.PNG?raw=true)

then we can do it accordingly

## Ansible for AWS
documentation: https://docs.ansible.com/ansible/latest/collections/amazon/aws/docsite/guide_aws.html

example: create key pair and save it
requirement: install boto3
```
- hosts: localhost
  gather_facts: False
  tasks:
    - name: create key pair
      ec2_key:
        name:sample
        region:us-east-1
      register:keyout

    - name: print key  # it will print key in command shell
      debug:
        var:keyout

    - name: save key  # it will save key in sample.pem file in present directory
      copy:
        content:"{{keyout.key.private_key}}"
        dest:./sample.pem
      when: keyout.changed
```

example: launch EC2

requirement: install aws collection
```
ansible-galaxy collection install amazon.aws
```

```
- name: start an instance 
  amazon.aws.ec2_instance:
    name: "public-compute-instance"
    key_name: "sample"
    #vpc_subnet_id: subnet-5ca1ab1e
    instance_type: t2.micro
    security_group: default
    #network_interfaces:
     # - assign_public_ip: true
    image_id: ami-123456  # get it from aws console
    exact_count: 1  # if we run multiple times. only 1 instance will create
    region: us-east-1
    tags:
      Environment: Testing
