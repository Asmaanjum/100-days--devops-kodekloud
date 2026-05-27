# Day 16 : install and configure Nginx as an LBR

## Task : 
1. install nginx on the LBR (load balancer) server if it is not already installed
2. configure load-balancing with the http context making useof all app servers ensure that you update only the main nginx configuration file located at /etc/nginx/nginx.conf
3. make sure you do not update the apacheport that is already define in the apache configuration on all app servers, also make sure apache service is up and running on all the app servers
4. once done, you can access the website by running curl http://stlb01:80 in the terminal

## step 1 : SSH to LBR server
'''ssh loki@stlb01'''

WHY:LBR server is where nginx runs loki is user for stlb01

## step 2 : install nginx
'''sudo yum install nginx -y'''

WHY : nginx acts as the load balancer -y auto confirms installation
RESULT:complete --nginx installed successfully

## step 3 : check apache port on app servers
'''ssh tony@stapp01
grep -i "^Listen" /etc/httpd/conf/httpd.conf
exit'''

'''ssh steve@stapp02
grep -i "^Listen" /etc/httpd/conf/httpd.conf
exit'''

'''ssh banner@stapp03
grep -i "^Listen" /etc/httpd/conf/httpd.conf
exit'''

WHY:we must know which port apache is listening on before configuring nginx never assume the port
RESULT:listen 8089 on all 3 servers

## step 4 : write nginx config
'''ssh loki@stlb01

cat > /tmp/nginx.conf << 'EOF'
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    upstream myapp {
        server stapp01:8089;
        server stapp02:8089;
        server stapp03:8089;
    }

    server {
        listen 80;
        location / {
            proxy_pass http://myapp;
        }
    }
}
EOF   '''

Why we write to /tmp first: Avoids sudo permission issues when using heredoc.
Config explanation:
.upstream myapp — defines the pool of backend servers
.server stapp01:8089 — each app server with its Apache port
.listen 80 — Nginx listens on port 80 (HTTP)
.proxy_pass http://myapp — forwards requests to the upstream pool

## step 5 : copy config to nginx
'''sudo cp /tmp/nginx.conf /etc/nginx/nginx.conf'''

WHY : the real config location is /etc/nginx/nginx.conf task requires updating only this file

## step 6 : test nginx config
'''sudo nginx -t'''

WHY : validates syntax before starting prevents crashes
EXPECTED RESULT : 
nginx:the configuration file syntax is ok
nginx:configuration file test is successful

## step 7 : start nginx
'''sudo systemctl restart nginx
sudo systemctl enable nginx'''

WHY RESTART:applies new config
WHY ENABLES:auto starts on reboot

## step 8 : start apache on all app servers
'''ssh tony@stapp01
sudo systemctl start httpd
exit'''

'''ssh steve@stapp02
sudo systemctl start httpd
exit'''

'''ssh banner@stapp03
sudo systemctl start httpd
exit'''

WHY:nginx forwards traffic to apache if apache is down--502 bad gateway error

## step 9 : verify
'''curl http://stlb01:80'''

WHY:test the full flow:client--nginx(port 80)--apache(port 8089)
EXPECTED RESULT:
welcome to xfusioncorp industries!


