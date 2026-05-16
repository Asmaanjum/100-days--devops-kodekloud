## Day 5 : selinux configuration on app server3

### task:
1.install the required selinux packages
2.permanently disable selinux for the time being;it will be re enabled after necessary configuration changes.
3.no need to reboot the server,as a scheduled maintenance reboot is already planned for tonight.
4.disregard the current status of selinux via the command line;the final status after the reboot should be disabled

### step 1:ssh into app server 3
ssh banner@stapp03

### step 2:install selinux packages
sudo yum install -y selinux-policy selinux-policy-targeted policycoreutils

### step 3:permanently disabled selinux
sudo vi /etc/selinux/config
#change SELINUX=enforcing to
 SELINUX=disabled

 ### step 4:verify the change
 cat/etc/selinux/config|grep SELINUX=

 #Output:SELINUX=disabled
