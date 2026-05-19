# Day 8 - Ansible Installation

## Task:
install ansible version 4.7.0 on jump host using pip3 so that all users can access it globally.

### step 1 : install ansible 4.7.0 via pip3
sudo pip3 install ansible==4.7.0

### step 2 : verify installation 
ansible --version

## output received
ansible [core 2.11.12]
executable location = /user/local/bin/ansible
python version = 3.9.19
jinja version = 3.1.6

## what i learned 
.sudo pip3 install =install globally for all users
.pip3 install without sudo = install only for current user 
.jump host acts as ansible controller
.ansible 4.7.0 uses ansible core 2.11.12
