# Day 10 : Linux Bash script

##  Task:
the production support team of xfusioncorp industries is working on developing some bash scripts to automate different dayto day tasks. one is to create a bash script for archiving website 
content files. they have a static website running on app server 3 in stratos datacenter,and theyneed to create a bash script named ecommerce_archives.sh which should accomplish the following
tasks.(also remember to place the script under the /scripts directory on app server 3).

a.create a zip archive named xfusioncorp_ecommerce.zip of /var/www/html/ecommerce directory

b.save the archive in the /archives/ directory on the app server 3.this is a temporary storage,as archives from this location will be cleaned on a weekly basis therefore,the archive should 
also be copied to the nautilus storageserver so it canbe retrieved later for validation purposes.

c.copy the created archive to the nautilus storage server in the /archieves/ location

d.please make sure script wont ask for password while copying the archive file.additionally,the respective server user (for example,tony in case of app server 1) must be able to run it

e.do not use sudo inside the script

note:install zip file manually



### Step 1: SSH into App Server 3
ssh banner@stapp03

### step 2. switch to root to install zip and create directories
sudo su -

### step 3.Install the zip package
yum install zip -y

### step 4. Create /archives and /scripts directories
mkdir -p /archives
mkdir -p /scripts

### step 5. Setup passwordless SSH to nautilus storage server
exit
ssh-keygen -t rsa -N "" -f ~/.ssh/id_rsa
ssh-copy-id natasha@ststor01

### step 6. Write the bash script
vi /scripts/ecommerce_archive.sh

'''bash
zip -r /archives/xfusioncorp_ecommerce.zip /var/www/html/ecommerce
scp /archives/xfusioncorp_ecommerce.zip natasha@ststor01:/archives/
bash'''

### step 7. Run the script
bash /scripts/ecommerce_archive.sh

### step 8. verify the archieve exists on both servers
#### on app server 3:
ls -l /archives/
#### on nautilus storage server:
ssh natasha@ststor01 ls -l /archives/

##  Key Learnings
- zip -r for recursive archiving
- ssh-keygen for key generation
- ssh-copy-id for copying keys
- scp for secure file transfer
