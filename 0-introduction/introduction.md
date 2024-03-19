# Ansible for Network engineers


Ansible is simple IT automation software that helps provision systems or network devices. 

Ansible helps automate repetitive tasks and provisioning systems. A running system contains different software versions and configuration files. Ansible automation captures all these changes; if a situation comes to bring a new system, it becomes a trivial task. A single Unix/Linux system is required for running Ansible, we refer to Linux host in the document.

## Installation and configuration of Ansible

Ansible is a Python module; use pip to install ansible. 

'''
pip3.9 install ansible --user
'''

Installs ansible in user home directory /home/userA/.local/bin/ansible
Generating configuration file for userA

'''
ansible-config init --disabled > .ansible.cfg
'''
Modify the line containing inventory in .ansible.cfg
'''
;inventory=/etc/ansible/hosts replace with
inventory=/home/userA/ansible/hosts
'''
Create a directory as below
'''
mkdir /home/userA/ansible
'''

Note: replace userA with your username.

## ssh
Ssh stands for secure shell; you should be familiar with it by now. Generate ssh keypair and copy public to remote host to ssh passwordless. The remote user also can elevate privilege (sudo access) to install packages.
Generation of an SSH key pair and copy the public key to the remote host is outside of this article.

# Inventory
Installation of Ansible, copying SSH public key to remote hosts, and arranging inventory are the only configurations required for starting the Ansible journey.

You probably need more time to arrange all your inventory. There are multiple servers to manage, ranging from database to front end. Arrange inventory in /home/userA/ansible/hosts file (as configured above).

    [database]
    192.168.2.17
    192.168.2.25
    
    [frontend]
    192.168.1.10
    192.168.1.11
    
    [webapp:children]
    database
    frontend

Grouping like above helps to apply common packages to webapp (all database and frontend servers) and specific packages to database and front servers.

Inventory like above assumes you have username same as userA, if username is different to userA, you need to modify inventory file. If ssh port is different need to update port number too.

## Playbooks
Let us start running few commands and as an extension write playbooks to automate installation of packages or setting configuration files.

Copy configuration file to all hosts in the inventory. 

```
ansible -i /home/userA/ansible/hosts webapp -mcopy  -b -a "src=/tmp/resolv.conf dest=/etc/resolv.conf"
```
1. -i /home/userA/ansible/hosts is an optional  if your inventory file matches with ansible.cfg file
2. webapp is coming from inventory file, it matches both children listed above
3. -mcopy This is module ansible using. There are multiple inbuilt modules are there, some modules we can install. This is copy module.
4. -b option is become, this makes user can get elevated privilege. 
5. src is about where the file is located, we have file locate in /tmp/resolv.conf, when running this command it copies /tmp/resolv.conf to all hosts and replaces /etc/resolv.conf
6. There are multiple problems with the above command. /tmp/resolv.conf file will be removed this command is not repetitive.

We will write playbook to run the above command and advantages of playbook.

```
name: Ansible Copy files local to Remote hosts
  hosts: webapp
  become: true
  tasks:
    - name: Copy resolv.conf
      copy:
        src: /home/userA/ansible/files/resolv.conf
        dest: /etc/resolv.conf
        owner: root
        group: root
        mode: 0644
```

That is all involved in writing ansible playbook. There are some questions here we will try to answer.
1. What happens if I run playbook twice?
2. I want list of all internal modules and modules available for my Network device I can download for?
3. I have specific requirement for type of host, if host OS is RedHat one location, debian I wanted to copy to different location.
4. I would like to copy file, but one line contains hostname. Should I create one file for each host?
5. I have different users for each host, how do I update inventory file?
