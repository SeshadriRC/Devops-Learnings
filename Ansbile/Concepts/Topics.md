- Inventory, create ansible inventory, install httpd, yum module ( Day 82 )
- file module ( Day 83 ), file creation/permission settting, owner and group change -> refer ( Day 88 )
- Copy data to other app servers ( Day 84 ), copy module
- Create files on app servers ( Day 85 ), file module, when module
- Ping Module ( Day 86 )
- Install package - zip ( Day 87 ) -> here we will use yum module
- Blockinfile Module - Day 88
- Ansbile-vault encrypt, ansible_become_password, ansible_become_method - Day 89
- ACL control - Day 90
- lineinfile module - Day 91
- Ansible role, galaxy - Day 92
- Playbooks
- Modules
   ansible_doc
   modules learned: service, lineinfile, command, user, firewalld, yum, package,service,shell
   Dynamic Inventory Plugin
   Module plugin
  ansible-inventory --list -i aws_inventory.py
   cisco.ios
  ec2_instance
- Variables
     registe,
- Conditionals (using when)
- loops  (using with_items)
- Ansible Roles




```
In this lab exercise you will use below hosts. Please note down some details about these hosts as given below :


student-node :- This host will act as an Ansible master node where you will create playbooks, inventory, roles etc and you will be running your playbooks from this host itself.


node01 :- This host will act as an Ansible client/remote host where you will setup/install some stuff using Ansible playbooks. Below are the SSH credentials for this host:


User: bob
Password: caleston123


node02 :- This host will also act as an Ansible client/remote host where you will setup/install some stuff using Ansible playbooks. Below are the SSH credentials for this host:


User: bob
Password: caleston123


Note: Please type exit or logout on the terminal or press CTRL + d to log out from a specific node.


```
