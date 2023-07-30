# NGINX Config in ubuntu

## Installing Nginx

Because Nginx is available in Ubuntu’s default repositories, it is possible to install it from these repositories using the apt packaging system.

Since this is our first interaction with the apt packaging system in this session, we will update our local package index so that we have access to the most recent package listings. Afterwards, we can install nginx:

      sudo apt update
      sudo apt install nginx

After accepting the procedure, apt will install Nginx and any required dependencies to your server.

## Start Nginx

To start using nginx

      systemctl start nginx

and To check his status 

      systemctl status nginx

this will give us the default page of nginx in the browser if we tray to use ip or domain.

finaly we enable nginx by:

      systemctl enable nginx

## Nginx config

The main file of nginx is `/etc/nginx/nginx.conf`that include file from `/etx/nginx/conf.d`.

      nano /etc/nginx/conf.d/domain.conf

this is an example of settings

      server {
              listen 80;
              listen [::]:80;
              server_name domain.com;

              proxy_set_header Host $http_host;
              proxy_set_header X-Real-IP $remote_addr;
              proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
              client_max_body_size 3M;

              location /api {
                      proxy_pass "http://localhost:3000";
                      proxy_http_version 1.1;
                      proxy_set_header Upgrade $http_upgrade;
                      proxy_set_header Connection 'upgrade';
                      proxy_set_header Host $host;
                      proxy_cache_bypass $http_upgrade;
              }

              location / {
                      root /var/www/domain;
                      index index.html index.htm;
                      proxy_http_version 1.1;
                      proxy_set_header Upgrade $http_upgrade;
                      proxy_set_header Connection '';
                      try_files $uri $uri/ /index.html;
              }
      }

So by this configuration nginx will listen to port 80 (domain) and will redirect every request to specific location for 
example `/api` will be passed to the `localhost:3000` and the `/` will be server an index html file located in `/var/www/domain`

## Restart ngInx

To check if the code you write is correct :

      nginx -t

Then restart nginx:

      systemctl restart nginx

## Check ports

To check which ports are listening :

      netstat -tpln
