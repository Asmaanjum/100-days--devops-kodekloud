# Day 7 - password-less SSH authentication

## task
set up password-less ssh from user 'thor' on jump host to all app servers through their sudo users.

## infrastructure 
|   server     |   hostname    |   user    |
|--------------|---------------|-----------|
|app server 1  | stapp01       | tony      |
|app server 2  | stapp02       |  steve    |
|app server 3  | stapp03       |  banner   |

### step 1 - generate ssh key on jump host 
ssh-keygen -t rsa
# press enter 3 times (no passphrase)

### step 2 - copy public key to all app servers
ssh-copy-id -o StrictHostKeyChecking=no tony@stapp01
ssh-copy-id -o StrictHostKeyChecking=no steve@stapp02
ssh-copy-id -o StrictHostKeyChecking=no banner@stapp03

### step 3 - verify password-less login 
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03

