# -
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
      #  listen 80 default backlog=2048;
        listen       443 ssl;
        server_name  www.rjkf001.com;

#        index  webapp/index.html;

     #   return  http://39.107.78.209/webapp;


 #       location =/ {
    #        try_files $uri $uri/ /webapp/index.html;
 #       }


        ssl_certificate      b.pem;
        ssl_certificate_key  b.key;

        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;

        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;

       # proxy_pass http://39.107.78.209:8082;
#       return  301 http://39.107.78.209:8082;

      #
      #          location / {
      #                      root   html;
      #                                  index  index.html index.htm;
      #                                          }

        #charset koi8-r;

        #access_log  logs/host.access.log  main;
        
        #root  html;
#        index  webapp/index.html webapp/index.htm;
       # index  webapp/index.html;

        location /webapp {
            try_files $uri $uri/ /webapp/index.html;
        }

        location / {
            proxy_pass http://39.107.78.209:8082;
        }

        location ~ ^/(images|javascript|js|css|flash|media|static)/ {
            root   /usr/local/jndnginx/html/webapp;
        }


        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


     #HTTPS server
    
    server {
        listen       80;
        server_name  www.9nengdai.com;

#        ssl_certificate      a.pem;
#        ssl_certificate_key  a.key;

#        ssl_session_cache    shared:SSL:1m;
#        ssl_session_timeout  5m;

#        ssl_ciphers  HIGH:!aNULL:!MD5;
      #  ssl_prefer_server_ciphers  on;

        location / {
	    #proxy_pass http://softtrade.top;
	    proxy_pass http://39.107.78.209:8889;
        }
    }

}
