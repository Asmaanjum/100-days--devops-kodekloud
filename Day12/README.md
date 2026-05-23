# Day 12 - apache service troubleshooting

## Task :
monitoring reported apache (httpd) unreacheable on port 5001 across app servers in stratos datacenter.

### Step 1 : check apache on jump host
'''sudo systemctl status httpd'''

why:first checked if apache was runninng.
result:failed - jump host doesn't use systemd.then i need to ssh into the actual app servers

### step 2 : SSH into stapp03
'''sudo systemctl status httpd'''

why:need to check apache directly on the app server
result:apache was stopped.

### step 3 : start and enable apache on stapp03
'''sudo systemctl start httpd
sudo systemctl enable httpd'''

why:
start - runs the service immediately
enables - makes it auto start on reboot
result:apache started,listening on port 5001

### step 4 : test stapp03 from jump host
'''curl http://stapp03:5001'''

why:verify apache is reachable from outside the server
result:got HTML response

### step 5 : SSH into stapp01
'''ssh tony@stapp01
sudo systemctl status httpd'''

why:check apache status on second server
result:apache failed to start - port already in use

### step 6 : find what is blocking port 5001
'''sudo ss -tlnp | grep 5001'''

why:ss shows which process is using a port
result:sendmail(pid=40605) was occupying 5001.

### step 7 : kill sendmail and stop the service 
'''sudo kill -9 40605
sudo systemctl stop sendmail
sudo systemctl disabled sendmail'''

why:
kill -9 -- force kills the blocking process
stop - stops send mail service
disable - prevents it from restarting on reboot
result : port 5001 is free 

### step 8 : start apache on stapp01
'''sudo systemctl start httpd
sudo systemctl enable httpd'''

why:now that port is free,start apache
result:apache running on port 5001

### step 9 : open firewall for stapp01
'''sudo iptables -I INPUT -p tcp --dport 5001 -j ACCEPT'''

why:firewall-cmd was not available.used iptables instead to allow incoming traffic on port 5001
result:port accessible from jump host

### step 10 : check stapp02
'''ssh steve@stapp02
sudo systemctl status httpd'''

why:verify third server as well
result:apache was already running but disabled

### step 11 : enable apache on stapp02
'''sudo systemctl enable httpd'''

why:service running but would not survive reboot
result:apache enabled

### final verification from jump host
'''curl http://stapp01:5001
curl http://stapp02:5001
curl http://stapp03:5001'''

why:confirm all 3 servers responds correctly
result:all returned Html response


