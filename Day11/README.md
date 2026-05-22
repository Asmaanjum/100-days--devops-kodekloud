# Day 11: install and configure tomcat server

## Task:
Deploy a java based web application usingapache tomcat on a linux server (kodekloud -nautilus project)

### step 1: login to jump host first
scp /tmp/ROOT.war steve@stapp02:/tmp/

### step 2: SSH into app server 2
ssh steve@stapp02

### step 3: switch to root
sudo su -

### step 4: install tomcat
yum install -y tomcat

### step 5: change tomcat port to 3001
vi /etc/tomcat/server.xml
find this line:
<connector port="8080"
change to:
<connector port="3001"

### step 6: Deploy ROOT.war
(remove default root folder)
rm -rf /usr/share/tomcat/webapps/ROOT
(copy ROOT.war to webapps)
cp /tmp/ROOT .war /usr/share/tomcat/webapps/ROOT.war

### step 7: start tomcat
systemctl start tomcat
systemctl enable tomcat
systemctl status tomcat

### step 8: verify
curl http://stapp02:3001

