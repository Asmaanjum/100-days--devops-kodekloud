# Day 9 - MariaDB Service Troubleshooting

## task
The Nautilus application in Stratos DC was unable to 
connect to the database. MariaDB service was down on 
the database server (stdb01).

### Step 1. SSH into database server:
ssh peter@stdb01
### step 2. Checked service status: 
sudo systemctl status mariadb
   - Status: inactive (dead), disabled
### step 3. Tried to start: 
sudo systemctl start mariadb → FAILED
### step 4. Checked logs: 
sudo journalctl -xeu mariadb.service
### step 5. Checked mariadb.log: 
sudo cat /var/log/mariadb/mariadb.log

##  Root Cause
`/run/mariadb/` directory had incorrect ownership.
MariaDB couldn't write its PID file → Permission denied.

##  Fix
sudo chown -R mysql:mysql /run/mariadb/
sudo systemctl enable mariadb
sudo systemctl start mariadb
