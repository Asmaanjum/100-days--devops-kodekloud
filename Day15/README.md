# Day 15 : setup ssl for nginx

## Task : 
1. install and configure nginx on app server 1 ,
2. on app server 1 there is a self signed ssl certificate andkey present at location /tmp/nautilus.crt and /tmp/nautilus.key. move them to some appropriate location and deploy the same in nginx
3. create an index.html file with content Welcome1 under nginx document root
4. for final testing try to access the app server 1 link (via hostname) from jump host using curl command for example: curl -Ik https://<app-server-name>/

## step 1 : SSH into app server 3
'''ssh banner@stapp03'''

WHY:we need to remotely access app server 3 to work on it. banner is the username for stapp03
RESULT:we are now inside stapp03 terminal prompt changes to banner@stapp03

## step 2 : install Nginx
'''sudo yum install -y nginx'''

WHY:nginx is not install by default we need it as our web server to serve content over HTTPS.yum is package manager for centos/rhel. -y means auto accept all prompts
RESULT:nginx gets downloaded and installed successfully

## step 3 : start and enable nginx
'''sudo systemctl start nginx'''
'''sudo systemctl enable nginx'''

WHY:
start-- runs nginx right now
enable--makes nginx start automatically every time server reboots
RESULT:nginx is running and will survive reboots

## step 4 : create ssl directory
'''sudo mkdir -p /etc/nginx/ssl'''

WHY:we need a secure folder to store ssl certificate and key. /etc/nginx/ is nginx's config directory. keeping certs here is standard practice.-p means create parent folders if they dont 
exist
RESULT:folder/etc/nginx/ssl/ is created

## step 5 : move ssl certificates
'''sudo mv /tmp/nautilus.crt /etc/nginx/ssl/
sudo mv /tmp/nautilus.key /etc/nginx/ssl/'''

WHY:the certs are temporarily in /tmp/ which is not secure ---anyone can access/tmp/.moving them to /etc/nginx/ssl/is safer and is the proper location for nginx to use them
RESULT:both files now live at
./etc/nginx/ssl/nautilus.crt
./etc/nginx/ssl/nautilus.key

## step 6 : create index.html
'''echo "Welcome!" | sudo tee /usr/share/nginx/html/index.html'''

WHY:the task requires an index.html file with content welcome! under nginx document root./usr/share/nginx/html/ is where nginx serves files from by default
RESULT:file created with content welcome!-- this is what users see when they visit the website

## step 7 : create SSL config
'''sudo bash -c 'cat > /etc/nginx/conf.d/ssl.conf << EOF
server {
listen 443 ssl;
server_name stapp03;
ssl_certificate /etc/nginx/ssl/nautilus.crt;
ssl_certificate_key /etc/nginx/ssl/nautilus.key;
root /usr/share/nginx/html;
index index.html;
}
EOF'   '''

WHY:we need to tell nginx:
.listen on port 443 (HTTPS port)
.use our SSL certificate and key
.serve files from /usr/share/nginx/html/
we use conf.d/ folder because nginx automatically includes all .conf files from this folder-- no need to edit the main nginx.conf
RESULT:file /etc/nginx/conf.d/ssl.conf is created with our HTTPS configuration

## step 8 : test nginx config
''' sudo nginx -t'''

WHY:before restarting we test for any syntax errors if config has errors and we restart,nginx will crash
RESULT:
nginx:the configuration file syntax is ok
nginx:configuration file test is successful

## step 9 : restart nginx
'''sudo systemctl restart nginx'''

WHY:Nginx needs to reload to apply our new ssl configuration
RESULT:nginx is now running with HTTPS enabled

## step 10 : exit and final test
'''exit
curl -Ik https://stapp03/

WHY:
.exit--goes back to jump host
.curl -Ik --sends HTTPS request to stapp03
     -I--shows only headers
     -k--ignores self-signed certificate warning
RESULT:HTTP/1.1200 OK
server:nginx/1.20.1
200 ok means everything is working perfectly
