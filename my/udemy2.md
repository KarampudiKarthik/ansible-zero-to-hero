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

