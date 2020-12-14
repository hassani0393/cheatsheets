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

#### Load Balancer

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
  proxy_http_version 1.1;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  proxy_set_header X-Real_IP $remote_addr;
  proxy_set_header Upgrade $htt_upgrade;
  proxy_set_header Connection "Upgrade";
  
  }
}  

#### Authentication

#### Nginx Mointoring Metrics

#### Logging

#### SSL

#### HAproxy

#### LAMP
it's for apache

#### LEMP
is for nginx

#### XAMP


