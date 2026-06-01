# Day 21 : set up git repository on storage server

## Task : 
1.utilize yum to install the git package on the storage server
2.create a bare repository named /opt/ecommerce.git (ensure exact name usage)

## step 1 : ssh into storage server
'''ssh natasha@ststor01'''

## step 2 : install git using yum
'''sudo yum install -y git'''

## step 3 : create the bare repository  
'''sudo git init --bare /opt/ecommerce.git'''

## step 4 : verify
'''ls /opt/ecommerce.git'''
