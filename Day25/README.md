# Day 25 : Git merge branches

## Task : 
1.the nautilus application devlopment team has been working on a project repository /opt/games.git. this repo is cloned at /usr/src/kodekloudrepos on storage server in stratos DC
  they recently shared the following requirements with devops team
2.create a new branch nautilus in /usr/src/kodekloudrepos/games repo from master and copy the /tmp/index.html file (present on storage server itself) into the repo further,add/commit
  this file in the new branch and merge back that branch into master branch.finally,push the changes to the origin for both of the branches

## step 1 : SSH into the storage server
'''ssh natasha@ststor01'''

WHY : to remotely access the storage server the repo lives
RESULT : successfully loged into ststor01

## step 2 : navigate to the cloned repo
'''cd /usr/src/kodekloudrepos/games'''

WHY : to go inside the games repository folder
RESULT : entered the games directory

## step 3 : git checkout master (failed)
'''git checkout master'''

WHY : to make sure we start from master branch
RESULT : error--"dubious ownership"--repo owned by different user

## step 4 : fix ownership error
'''git config --global --add safe.directory /usr/src/kodekloudrepos/games'''

WHY : git blocks access when folder is owned by another user -- this adds an exception
RESULT : error bypassed,git allowed to proceed

## step 5 : checkout master with sudo (Failed again)
'''git checkout master'''

RESULT : error--"permission denied" on .git/index.lock

## step 6 : use sudo for git
'''sudo git checkout master'''

WHY : the repo folder is owned by root so normal user cant write--sudo gives admin power
RESULT : "already on master"--successfully on master branch

## step 7 : create new branch
'''sudo git checkout -b nautilus'''

WHY : task requires a new branch called nautilus created from master
RESULT : switched to new branch nautilus

## step 8 : copy file (FAILED TWICE)
'''sudo cp /tmp/index.html'''

RESULT : error--missing destination file operand

## step 9 : copy file (FIXED)
'''sudo cp /tmp/index.html /usr/src/kodekloudrepos/games/index.html'''

WHY : full destination path needed because shortcut ./didnt work
RESULT : index.html appeared confirmed by ls showing index.html info.txt welcome.txt

## step 10 : stage the file
'''sudo git add index.html'''

WHY : tell git to track this new file before saving
RESULT : file staged successfully

## step 11 : commit the file
'''sudo git commit -m "Add index.html" '''

WHY : permanently save the snapshot with a message
RESULT : [nautilus 0480b08] add index.html--1 file changed,1 insertion

## step 12 : push nautilus branch
'''sudo git push origin nautilus'''

WHY : upload nautilus branch to github/origin
RESULT : [new branch] nautilus -- nautilus-- pushed to /opt/games.git

## step 13 : switch back to master
'''sudo git checkout master'''

WHY : need to be on master before merging
RESULT : switched to branch master

## step 14 : merge nautilus into master
'''sudo git merge nautilus'''

WHY : bring the index.html changes into master branch
RESULT : fast forward merge--index.html | 1+--1 file changed

## step 15 : push master to origin
'''sudo git push origin master'''

WHY : upload the updated master branch to origin
RESULT : master --> master--task fully completed
