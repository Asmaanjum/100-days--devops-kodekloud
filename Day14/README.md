# Day 14 : Linux process troubleshooting

## Task : apache (httpd) was failing to start on stapp01 in a 3-server production environment.

### step 1 : check apache status
'''sudo systemctl status httpd'''

WHY : check if apache is running or failed
RESULT : showed failed -- apache couldn't start,exit-code error

## step 2 : check for port 5000
'''sudo ss -tlnp | grep 5000'''

WHY : find what process is using port 5000
-t = TCP connections 
-l = listening ports only
-n = show numbers not names
-p = show process name/PID
RESULT : revealed sendmail(PID=11790) was occupying port 5000--this was the smoking gun

## step 3 : freeding port 5000
'''sudo systemctl stop sendmail'''

WHY : kill the process blocking port 5000
RESULT : sendmail stopped,port 5000 freed

## step 4 : disabled sendmail
'''sudo systemctl disable sendmail'''

WHY : prevent sendmail from starting again after reboot
RESULT : sendmail wont auto-start next boot

## step 5 : apache is configured to port 5000
'''sudo grep "Listen" /etc/httpd/conf/httpd.conf'''

WHY : verify apache is configured to use port 5000
RESULT : showed listen 5000 - config was correct,no change needed

## step 6 : start apache 
'''sudo systemctl start httpd'''

WHY : start apache now that port is free
RESULT : apache started successfully

## step 7 : enable the apache
'''sudo systemctl enable httpd'''

WHY : make apache auto-start on every reboot
RESULT : created systemlink- service now persistent

## step 8 : checking status 
'''sudo systemctl status httpd'''

WHY : confirm apache is actually running
RESULT : active (running),listening on port 5000
