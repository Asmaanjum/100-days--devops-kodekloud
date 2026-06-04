# Day 24 : Git create branches

## Task :
1.on storage server in stratos Dc create a new branch xfusioncorp_official from master branch in /usr/src/kodekloudrepos/official git repo
2.please do not try to make any changes in the code

## step 1 : ssh into the storage server
'''ssh natasha@ststor01'''

Why : connected to remote storage server securely

## step 2 : navigate to repository
'''cd /usr/src/kodekloudrepos/official'''

WHY : moved into the git repository folder

## step 3 : fix ownership error
'''git config --global --add safe.directory /usr/src/kodekloudrepos/official'''

WHY : git blocked access because directory was owned by different user.this command tells git to trust this directory

## step 4 : checkout master with sudo
'''sudo git checkout master'''

WHY : normal user didnt have permission sudo runs command as admin/root user

## step 5 : create new branch
'''sudo git checkout -b xfusioncorp_official'''

WHY : -b flag creates a NEW branch and switches to it at the same time

## step 6 : verify
'''git branch'''

## RESULT : confirmed*xfusioncorp_official was created
