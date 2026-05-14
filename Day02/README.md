# Day 02 - create user with expiry date
## Task
create a temporary user 'kareem' on app server 2 with expiry date '2027-04-15'.
## commands
### step 1: SSH app server 2
''' bash
ssh steve@stapp02
'''

###step 2: create user with expiry date
'''bash
sudo useradd -e 2027-04-15 kareem
'''

###step 3: verify
'''bash
sudo chage -l kareem
'''


Command         |    Description
---------------|---------------
useradd        | command used to create new user  kareem
-e             |sets the account expiry date(YYYY-MM-DD format)
kareem          | username in lower case
chage -l        |list user password and account expiry info

