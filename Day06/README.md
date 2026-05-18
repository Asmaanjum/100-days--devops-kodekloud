# Day 6 - create a corn job

## task: install cronie,start crond service and add cron job on all app server

### step 1: connect to app server
ssh tony@stapp01

### step 2:install corn package
sudo yum install -y cronie

### step 3:start corn service
sudo systemctl start crond

### step 4:auto start on reboot
sudo systemctl enable crond

### step 5:verify corn job
sudo crontab -l

## cron job added:
*/5 * * * * echo hello > /tmp/cron_text
runs every 5 minutes for root user

