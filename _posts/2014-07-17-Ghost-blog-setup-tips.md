---
layout: post
title:  "Ghost blog setup tips"
date:   2014-07-17 17:29:58+1000
categories: nodejs blog
tags: blog nodejs
---
There are tips for setting up a ghost blog on a linux server. We can adjust the IP-address and port in config.js

#### Forever start app
You can use forever to run Ghost as a background task. forever will also take care of your Ghost installation and it will restart the node process if it crashes.

To install forever type 
               
    npm install forever -g
To start Ghost using forever from the Ghost installation directory type 
          
    NODE_ENV=production sudo forever start index.js
To stop Ghost type 

    forever stop index.js
To check if Ghost is currently running type 

    forever list
Reference: http://docs.ghost.org/installation/deploy/ 

#### Nginx configuration
The only thing you should need to change is the domain name.

    server {
    listen 0.0.0.0:80;
    server_name *your-domain-name*;
    access_log /var/log/nginx/*your-domain-name*.log;

    location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header HOST $http_host;
        proxy_set_header X-NginX-Proxy true;

        proxy_pass http://127.0.0.1:2368;
        proxy_redirect off;
      }
    }
    
Reference: https://www.digitalocean.com/community/tutorials/how-to-host-ghost-with-nginx-on-digitalocean    

#### Google Analytics
Open default.hbs inside /content/themes/casper in a text editor
Add your GA tracking code before the &lt;/head&gt;