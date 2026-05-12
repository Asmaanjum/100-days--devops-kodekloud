# Day 01  - linux user management 
## Task
create a non interactive user on app server 3
## commands used 
ssh banner@stapp03
sudo useradd -s /sbin/nologin mariyam
grep mariyam /etc/passwd
## what i learned 
- ssh = remotely connect to a server
- sudo = run as admin
- useradd = create new user
-  -s = assign shell to user
-  /sbin/nologin = blocks login
-  /etc/passwd = stores all linux users
## status:  completed
