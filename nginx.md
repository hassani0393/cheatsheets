### Nginx
___
#### Web Server


#### Nginx Deep Dive
Worker Proccess model with threads triggered by events, supports dynamic third party modules.
/etc/nginx is the directory for configurations of nginx.
main server configuration: /etc/nginx/nginx.conf
virtual hosts go into conf.d directory in the .conf files.

worker_processes should be equal to cpu cores for a dedicated server. auto will detect number of cpu cores and sets it equal to that.

total number of connections = worker_processes * worker_connections = number of clients you can server.

su -s /bin/sh -c "ulimit -Hn" : hard limit for worker connections.
su -s /bin/sh -c "ulimit -Sn" : Soft limit for worker connections.

worker_rlimit_nofile 2048; : sets the soft limit for worker processes. used to increase the limit without restarting the main process.

keep-alive connections allows a single tcp connection to stay open for more than one req.
keepalive_timeout 75; : sets it to 75s. 
keepalive_requests 100; :sets the number of requests that can be made in a single tcp keepalive connection.
To keep-alive with proxy servers(upstream servers), we use keepalive directive in upstream block to set max number of idle keepalive connections to upstream servers that are preserved in the cache of each worker process.

Content compression: 
gzip on;

gzip_types text/plain text/css application/x-javascript application/javascript text/xml application/xml application/xml+rss text/javascript image/x-icon image/bmp image/svg+xml;

gzip_disable regex;

gzip_min_length 20;

gzip_proxied; (study later.)

gunzip on; :if the client requests unziped file it will unzip it.

PageSpeed by Google: 
cd opt
bash <(curl -f -L -sS https://ngxpagespeed.com/install) --help :flags for installation of pagespeed.

bash <(curl -f -L -sS https://ngxpagespeed.com/install) -b . --dynamic-module --ngx-padespeed-version latest-stable : installation without downing the server.

./configure --with-compat --add-dynamic-module=./incubator-pagespeed-ngx-latest-stable

make modules

cp objs/ngx_padespeed.so /etc/nginx/modules/


##### HTTP
Is a request-response protocol between a client and server.

language of the Internet. Application layer protocol built upon TCP/IP model. Defines the format for sending info between web clients and web servers.
HTTPS is the same only with encrypted communication between web clients and servers using TLS/SSL.
Default protocol in the URL: http 80, https 443. The resource address is relative to virtual host root.
URI: path portion of the URL.

HTTP Request Circle:
1. The browser requests an HTML page. The server returns an HTML file.
2. The browser requests a style sheet. The server returns a CSS file.
3. The browser requests an JPG image. The server returns a JPG file.
4. The browser requests JavaScript code. The server returns a JS file.
5. The browser requests data. The server returns data (in XML or JSON).

XMLHttpRequest Object (XHR):
All browsers have a built-in one. It's a JavaScript object that is used to transfer data between a web browser and a web server.
XHR is often used to request and recieve data for the purpose of modifying a web page.
IT IS NOT USED WITH HTTP 
HTTP Request : URL, Method type, Headers, Body

HTTP status message: the server always return one for every request. The most common is 200 OK. 




HTTP METHODS
GET : retrieves data from the server, one of the most common HTTP methods. GET requests can be cached. GET requests remain in the browser history. GET requests can be bookmarked. GET requests should never be used when dealing with sensitive data. GET requests have length restrictions. GET requests are only used to request data and not to modify.

POST : submits data to the server to create/update a resource. The data sent to the server with POST is stored in the request body of the HTTP request.
POST is never cached. POST does not remain in the browser history. POST requests cannot be bookmarked. POST requests have no length restriction on data.

PUT : update data already on the server. 

DELETE : deletes data from the server

HTTP Response : Status code, Headers, Body

HTTP2 in NGINX: To enable http2 in nginx content must be served in ssl.
server {
  listen 443 ssl http;
  server_name_;
  root /usr/share/nginx/html;

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

Syntax:	stub_status;
Default:	—
Context:	server, location
The basic status information will be accessible from the surrounding location.

In versions prior to 1.7.5, the directive syntax required an arbitrary argument, for example, “stub_status on”.
Data
The following status information is provided:

Active connections
The current number of active client connections including Waiting connections.
accepts
The total number of accepted client connections.
handled
The total number of handled connections. Generally, the parameter value is the same as accepts unless some resource limits have been reached (for example, the worker_connections limit).
requests
The total number of client requests.
Reading
The current number of connections where nginx is reading the request header.
Writing
The current number of connections where nginx is writing the response back to the client.
Waiting
The current number of idle client connections waiting for a request.
Embedded Variables
The ngx_http_stub_status_module module supports the following embedded variables (1.3.14):

$connections_active
same as the Active connections value;
$connections_reading
same as the Reading value;
$connections_writing
same as the Writing value;
$connections_waiting
same as the Waiting value.


#### Logging
log_module is used for logging conf. consisting of log_format and access_log directive !not for error loging.
error_log : sets the file and logging level. comes from core module. 

adding $host to the log format will allow us to see to which server the request has been sent.

 we can connect the logging to the system's syslog. vim conf.d -> 
 access_log syslog:server=uix:/dev/log main;
 
 
#### SSL

Generating a self-signed certificate.

mkdir /etc/nginx/ssl

openssl req -x509 -nodes -days 365  -newkey rsa:2048 -keyout /etc/nginx/ssl/private.key  -out /etc/nginx/ssl/public.pem
Generating a 2048 bit RSA private key

#### HAproxy Load Balancer

Benefits and drawbacks of using HAProxy
HAProxy is another open source load balancing solution. As it is a single-purpose solution in that it only offers load-balancing capabilities, it is much more focused on that one aspect compared to Nginx. Below are a few benefits and drawbacks to using HAProxy.

Benefits:
Provides a comprehensive list of 61 different metrics. See section 9 for a full list of available statistics
The status page is much more detailed and user-friendly as compared to Nginx's
Easily able to integrate with third party monitoring services (e.g. Datadog)

Drawbacks:
Does not provide other features that Nginx offers such as web server capabilities
HAProxy is quite thorough in terms of metrics it provides. Of course, since it is only a load balancing software you can't use it for other purposes as you can with Nginx.



#### LAMP
it's for apache

#### LEMP
is for nginx

#### XAMP


