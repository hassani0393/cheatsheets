# Nginx Cheat Sheet 
* [Nginx Documentation](http://nginx.org/en/docs/)

## Extracted Info From Nginx Deep Dive Course 

<p> /etc/nginx is the directory for configurations of nginx.

* Main server configuration directory: /etc/nginx/nginx.conf

* Virtual hosts go into conf.d directory in the .conf files.

**worker_processes** should be equal to cpu cores for a dedicated server. auto will detect number of cpu cores and sets it equal to that.

total number of connections =     **worker_processes** * **worker_connections** =   **Number** of clients you can serve.


    su -s /bin/sh -c "ulimit -Hn" // hard limit for worker connections.
    su -s /bin/sh -c "ulimit -Sn" // Soft limit for worker connections.

sets the soft limit for worker processes. used to increase the limit without restarting the main process.

    worker_rlimit_nofile 2048;
 
**keep-alive** connections allows a single tcp connection to stay open for more than one req.

Sets it to 75s. 
    
    keepalive_timeout 75; 


 Sets the number of requests that can be made in a single tcp keepalive connection.

    keepalive_requests 100;

<p>To keep-alive with proxy servers(upstream servers), we use keepalive directive in upstream block to set max number of idle keepalive connections to upstream servers that are preserved in the cache of each worker process.</p>

## Control Signals
syntax for sending a signal to master process.

    nginx -s <SIGNAL>

* quit – Shut down gracefully
* reload – Reload the configuration file
* reopen – Reopen log files
* stop – Shut down immediately (fast shutdown)

## A Sample Config File with Multiple Contexts

    user nobody; // a directive in the 'main' context

    events {
        // configuration of connection processing
    }


    http {
       // Configuration specific to HTTP and affecting all virtual servers  

        server {
            // configuration of HTTP virtual server 1       
            location /one {
                // configuration for processing URIs starting with '/one'
            }
            location /two {
                // configuration for processing URIs starting with '/two'
            }
        } 
        
        server {
            //y configuration of HTTP virtual server 2
        }
    }

    stream {
        // Configuration specific to TCP/UDP and affecting all virtual servers
        server {
            // configuration of TCP virtual server 1 
        }
    }


## HTTP
 
<br>

### HTTP2 in NGINX
To enable http2 in nginx content must be served in ssl. This will activate it.

        server { 
        listen 443 ssl http;
        server_name_;
        root /usr/share/nginx/html;
        }
## Location Configuration
    location ~ .jpg {
           return 403;
    }



## Load Balancer

<p>Upstream module is used for load balancing.
if you don't specify anything you get round robin load balancing.</p>

    upstream backend //name of the particular group of servers.
    {    
        server backend1.example.com       weight=5 max_conns=100; // prioritize this server. stablish maximum of 100 connections.
        server backend2.example.com:8080  max_fails=3 fail_timeout=20s; //if in 20 seconds 3 fails happens, downs the server, called passive health checking.
        server unix:/tmp/backend3;

        server backup1.example.com:8080   backup; //this server is marked as backup.
        server backup2.example.com:8080   backup;
    }

    server {
        location / {
            proxy_pass http://backend;
        }   
    }   

___
<p>Hash is determining where trafic shoud go based on a set keyword.</p>

    upstream photos {
        hash $request_uri;
        server 127.0.0.1:3000;
        server 127.0.0.1:3100;
        server 127.0.0.1:3101;
    }
___

<p>ip_hash load balancing method where requests are distributed between servers based on client addresses.</p>
    
    upstream allbackend {
        ip_hash;
        server 127.0.0.1:2222;
        server 127.0.0.1:3333;
        server 127.0.0.1:4444;
        server 127.0.0.1:5555;
    }


___

## Proxy Pass

    Server {
        listen 80;
        server_name photos.example.com;
  
    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;     // using http 1.1 for having keepalive connections. 
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Real_IP $remote_addr;
        proxy_set_header Upgrade $htt_upgrade;
        proxy_set_header Connection "Upgrade";
  
        }
    }  

## Authentication
    server {
         listen 80 default_server;
        server_name _;
         root /usr/share/nginx/html;

         location = /admin.html {
         auth_basic "Login Required";
         auth_basic_user_file /etc/nginx/.htpasswd;
        }

        error_page 404 /404.html;
        error_page 500 501 502 503 504 /50x.html;
    }
<p>Install or verify httpd-tools.<p>

## Nginx Mointoring Metrics

Module used for monitoring: **ngx_http_stub_status_module**

<code>Syntax:	stub_status;<br>
Default:	—<br>
Context:	server, location<br>
The basic status information will be accessible from the surrounding location.
</code>

In versions prior to 1.7.5, the directive syntax required an arbitrary argument, for example, “stub_status on”.

## Data
The following status information is provided:

### Active connections
The current number of active client connections including Waiting connections.
### Accepts
The total number of accepted client connections.
### Handled
The total number of handled connections. Generally, the parameter value is the same as accepts unless some resource limits have been reached (for example, the worker_connections limit).
### Requests
The total number of client requests.
Reading
The current number of connections where nginx is reading the request header.
### Writing
The current number of connections where nginx is writing the response back to the client.
### Waiting
The current number of idle client connections waiting for a request.
## Embedded Variables
The ngx_http_stub_status_module module supports the following embedded variables (1.3.14):

#### **$connections_active**
same as the Active connections value;
#### **$connections_reading**
same as the Reading value;
#### **$connections_writing**
same as the Writing value;
#### **$connections_waiting**
same as the Waiting value.

### Passive Health Checks
    upstream backend {
        server backend1.example.com;
        server backend2.example.com max_fails=3 fail_timeout=30s;
    }
* Active health check is only in NGINX Plus. 

### NGINX Amplify
Is a SaaS tool that you can use to monitor up to five servers for free.<br>
Visualizes NGINX performance, and monitors the OS, PHP‑FPM, Docker containers, and more. A unique feature in Amplify is a static analyzer for NGINX configuration that provides recommendations for making the configuration more secure and efficient.

## Logging
<p>log_module is used for logging configuration, consisting of log_format and access_log directive.<br>

<p>error_log : Sets the file and logging level. Comes from the core module.</p> 

    error_log  /var/log/nginx/error.log warn;


Adding $host to the log format will allow us to see to which server the request has been sent.

<p>We can connect the logging to the system's syslog.</p>

    vim conf.d -> access_log syslog:server=uix:/dev/log main;
 
 
## SSL

Generating a 2048 bit RSA private key.

    mkdir /etc/nginx/ssl

    openssl req -x509 -nodes -days 365  -newkey rsa:2048 -keyout /etc/nginx/ssl/private.key  -out /etc/nginx/ssl/public.pem


## HAproxy Load Balancer

<p>HAProxy is another open source load balancing solution. As it is a single-purpose solution in that it only offers load-balancing capabilities, it is much more focused on that one aspect compared to Nginx. Below are a few benefits and drawbacks to using HAProxy.
</p>

### Benefits
<p>Provides a comprehensive list of 61 different metrics. See 
The status page is much more detailed and user-friendly as compared to Nginx's
Easily able to integrate with third party monitoring services (e.g. Datadog)</p>

### Drawbacks
<p>Does not provide other features that Nginx offers such as web server capabilities.
HAProxy is quite thorough in terms of metrics it provides. Of course, since it is only a load balancing software you can't use it for other purposes as you can with Nginx.</p>



## LAMP
It stands for Linux Apache Mysql PHP

## LEMP
It stands for Linux nginx MySquel PHP

## XAMP
XAMP is a free and open-source cross-platform web server solution stack package developed by Apache Friends, made of Apache HTTP Server, MariaDB database, and interpreters for scripts written in the PHP and Perl programming languages.

