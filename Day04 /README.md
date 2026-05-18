# Day 04 - script execution permissions

## task description
grant executable permission to '/tmp/xfusioncorp.sh' on app server 1 for all users in stratos datacenter

### step 1 : ssh into app server 1
ssh tony@stapp01
### step 2 : grant permissions to all users
sudo chmod 755 /tmp/xfusioncorp.sh
### step 3 :verify 
ls -l/tmp/xfusioncorp.sh

## expected output
-rwxr-xr-x 1 root root 40 /tmp/xfusioncorp.sh

## what i learned 
chmod 755=owner(rwx)group(r-x)others(r-x)
use sudo for permission changes on root owned files
