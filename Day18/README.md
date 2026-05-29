# Day 18 : Install and configure DB server

## Task : 
a.install/configure mariaDB server
b.create a database named kodekloud_db8
c.create a user called kodekloud_tim and set its password to GyQkFRVNr3
d.grant full permissions to user kodekloud_tim on database kodekloud_db8

## step 1 : ssh into db server
'''ssh peter@stdb01'''

## step 2 : install and start MariaDB
'''sudo yum install -y mariadb-server
   sudo systemctl start mariadb
   sudo systemctl enable mariadb
   sudo systemctl status mariadb'''

## step 3 : access mariaDB as root
'''sudo mysql -u root'''

WHY SUDO : mariadb 10.5 uses unix_socket authentication by default the root account is tied to the OS root user,so regular users must use sudo to access it without a password

## step 4 : SQL commands
--create the database
CREATE DATABASE kodekloud_db8;
--create the user
CREATE USER 'kodekloud_tim'@'%' IDENTIFIED BY 'GyQkFRVNr3';
--Grant full permissions 
GRANT ALL PRIVILEGES ON kodekloud_db8.* TO  'kodekloud_tim'@'%';
