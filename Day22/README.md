# Day 22 : Clone Git Repository on Storage Server

## Task : 
1.the repository to be cloned is located at /opt/media.git
2.clone this git repository to the /usr/src/kodekloudrepos directory perform this task using the natasha user and ensure that no modifications are made to the repository or existing
directories,such as changing permissions or making unauthorized alterations

## step 1 : ssh to the storage server
'''ssh natasha@ststor01'''

## step 2 : clone the repository
'''git clone /opt/media.git /usr/src/kodekloudrepos/media'''

## step 3 : verify
'''ls /usr/src/kodekloudrepos/'''

## result :
successfully cloned the empty repository to the storage server as natasha user
