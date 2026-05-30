# Day 19 : install and configure web application

## Task : 
a.install httpd package and dependencies on app server 1
b.apache should serve on port 8088
c.there are two websites backups /home/thor/official and /home/thor/cluster on jump_host setthem up on apache in a way that official should work on the link http://localhost:8088/official/ 
and cluster should work on link http://localhost:8088/cluster/ on the mentioned app server
d.once configured you should be able to access the website using curl command on the respective app server ,i.e curl http://localhost:8088/official/ and curl http://localhost:8088/cluster/

## step 1 : ssh from jump-host to stapp01
'''ssh tony@stapp01'''

WHY : we always start on jump-host stapp01 is not directly accessible
RESULT : now logged into app server as tony

## step 2 : install apache on stapp01
'''sudo yum install -y httpd'''

WHY : httpd is apache web server for CentOS/RHEL
RESULT : apache+all dependencies installed

## step 3 : change port to 8088
'''sudo vi /etc/httpd/conf/httpd.conf'''
find listen 80-->change to listen 8088

WHY : task requires port 8088 not default 80
RESULT : apache will serve on port 8088

## step 4 : open terminal 2 (stays on jump-host)
terminal 2 auto connects to jump-host

#### step 1 - CHECK THE FILES EXIST:
'''ls /home/thor/official'''

#### step 2 - FIND STAPP01'S IP:
'''cat /etc/hosts'''

#### step 3 - COPY FILES FROM JUMP-HOST TO STAPP01:
'''scp -r /home/thor/official tony@10.244.81.38:/tmp/
scp -r /home/thor/cluster tony@10.244.81.38:/tmp/'''

WHY : website backup files exist only on jump-host
RESULT : files transferred to stapp01's /tmp/ folder

## step 5 :back on terminal 1 (stapp01)
'''sudo mkdir -p /var/www/html/official
sudo mkdir -p /var/www/html/cluster
sudo cp -r /tmp/official/* /var/www/html/official/
sudo cp -r /tmp/cluster/* /var/www/html/cluster/'''

WHY : apache serves file from /var/www/html
RESULT : website files in correct location

## step 6 : create apache config
'''sudo vi /etc/httpd/conf.d/websites.conf'''
APACHE:
<VirtualHost *:8088>
DocumentRoot /var/www/html
Alias /official/ /var/www/html/official/
<Directory /var/www/html/official/>
Require all granted
</Directory>
Alias /cluster/ /var/www/html/cluster/
<Directory /var/www/html/cluster/>
Require all granted
</Directory>
</VirtualHost>

WHY : alias maps URL path to folder location
RESULT : both URLs route to correct folders

## step 7 : start and enable apache
'''sudo systemctl start httpd
sudo systemctl enable httpd'''

WHY : 
start = run apache now
enable = auto start on server reboot
RESULT : apache running on port 8088

## step 8 : verify both sites
'''curl http://localhost:8088/official/
curl http://localhost:8088/cluster/

RESULT : official--> this is a sample page for our official website
         cluster--> this is a sample page for our cluster website
         
