### Nginx
___
#### Web Server


#### Nginx Deep Dive
Worker Proccess model with threads triggered by events, supports dynamic third party modules.
/etc/nginx is the directory for configurations of nginx.
main server configuration: /etc/nginx/nginx.conf
virtual hosts go into conf.d directory in the .conf files.




##### HTTP
language of the Internet. Application layer protocol built upon TCP/IP model. Defines the format for sending info between web clients and web servers.
HTTPS is the same only with encrypted communication between web clients and servers using TLS/SSL.
Default protocol in the URL: http 80, https 443. The resource address is relative to virtual host root.
URI: path portion of the URL.

#### Location Configuration
using location and regex we can put different directives for accessing certain location.
location ~ .jpg {
                        return 403;
}



#### Load Balancer
upstream module is used for load balancing.

if you don't specify anything you get round robin load balancing.

upstream backend {    : name of the particular group of servers.
    server backend1.example.com       weight=5 max_conns=100; : prioritize this server. stablish maximum of 100 connections.
    server backend2.example.com:8080  max_fails=3 fail_timeout=20s; :if in 20 seconds 3 fails happens, downs the server, called passive health checking.
    server unix:/tmp/backend3   ;

    server backup1.example.com:8080   backup; :this server is marked as backup.
    server backup2.example.com:8080   backup;
}

server {
    location / {
        proxy_pass http://backend;
    }
}

___
hash is determining where trafic shoud go based on a set keyword.

upstream photos {
    hash $request_uri;

server 127.0.0.1:3000;
server 127.0.0.1:3100;
server 127.0.0.1:3101;


}
___

ip_hash load balancing method where requests are distributed between servers based on client addresses.

upstream allbackend {
    ip_hash;
    server 127.0.0.1:2222;
    server 127.0.0.1:3333;
    server 127.0.0.1:4444;
    server 127.0.0.1:5555;
}


##### Traditional LB

##### HTTP/HTTPS

##### Application Load Balancing (ALB)



___

#### Proxy Pass
Server {
  listen 80;
  server_name photos.example.com;
  
  location / {
  proxy_pass http://127.0.0.1:3000;
  proxy_http_version 1.1;     : using http 1.1 for having keepalive connections. 
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  proxy_set_header X-Real_IP $remote_addr;
  proxy_set_header Upgrade $htt_upgrade;
  proxy_set_header Connection "Upgrade";
  
  }
}  

#### Authentication
Install or verify httpd-tools.

#### Nginx Mointoring Metrics

#### Logging
log_module is used for logging conf. consisting of log_format and access_log directive !not for error loging.
error_log : sets the file and logging level. comes from core module. 

adding $host to the log format will allow us to see to which server the request has been sent.

 we can connect the logging to the system's syslog. vim conf.d -> 
 access_log syslog:server=uix:/dev/log main;
 
 
#### SSL
ssl module
#### HAproxy

#### LAMP
it's for apache

#### LEMP
is for nginx

#### XAMP


