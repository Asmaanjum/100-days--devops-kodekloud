# Day 20 : configure Nginx + PHP-FPM using Unix Sock

## Task : 
a.install nginx on app server 1, configure it to use port 8095 and its document root should be /var/www/html
b.install php-fpm version 8.1 on app server 1,it must use the unix socket /var/run/php-fpm/default.sock (create the parent directories if dont exist)
c.configure php-fpm and nginx to work together
d.once configured correctly, you can test thewebsite using curl http://stapp01:8095/index.php command from jump host
NOTE:we have copied two files,index.php and info.php, under /var/www/html as part of the PHP-based application setup please do not modify these files

## step 1 : ssh into server
'''ssh tony@stapp01'''

WHY : to enter the app server from jump host

## step 2 : install nginx
'''sudo yum install -y nginx'''

WHY : nginx is the web server 
ERROR I GOT:
. first tried:sudo apt install nginx
. error:apt command not found
. reason:centos uses yum,not apt
.fix:used sudo yum instead

## step 3 : configure Nginx
'''sudo vi /etc/nginx/nginx.conf'''

CHANGES MADE:
.listen 80 -> listen 8095(changed port)
.root/usr/share/nginx/html -> /var/www/html
.added PHP-FPM location block

ERROR I GOT:
error:location"/404.html"is outside location".php$"
reason:missing closing } bracket
fix:added proper closing bracket

## step 4 : install PHP-FPM 8.1
'''sudo yum install -y http://rpms.remirepo.net/enterprise/remi-release-9.rpm
   sudo yum module reset php -y
   sudo yum module enable php:remi-8.1 -y
   sudo yum install -y php-fpm'''

WHY:PHP-FPM processess php files
ERROR I GOT:
.first tried:remi-release-7.rpm
.error:nothing provides epel-release=7
.reason:server is el9 not el7
.fix:used remi-release-9.rpm

## step 5 : configure PHP-FPM socket
'''sudo vi /etc/php-fpm.d/www.conf'''
changed:listen=/var/run/php-fpm/default.sock

WHY:socket is the pipe between nginx and php

## step 6 : start services
'''sudo mkdir -p /var/run/php-fpm
   sudo systemctl start php-fpm
   sudo systemctl enable php-fpm
   sudo systemctl restart nginx'''

FINAL RESULT
curl htttp://stapp01:8095/index.php
output:welcome to xfusioncorp industries!

   

