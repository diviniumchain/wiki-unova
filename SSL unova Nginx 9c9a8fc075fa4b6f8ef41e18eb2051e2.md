# SSL unova Nginx

# Launch nginx config

```yaml
server {
        listen 80 default_server;
        server_name develop.***.io;
        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;
        #return 301 https://develop.unova.io$request_uri;

         location /unova/ {
                proxy_pass http://localhost:8090;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
        }

        location /api/ {
                proxy_pass http://localhost:7000;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
        }

}
server {
        listen 443 ssl;

        server_name develop.unova.io;
        #return 301 https://develop.***.io$request_uri;
        root /var/www/html;

        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html;
        ssl_certificate /etc/nginx/ssl/***.crt;
        ssl_certificate_key /etc/nginx/ssl/***.key;

        location /api/ {
                proxy_pass http://localhost:7079;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
        }
        location /unova/ {
                proxy_pass http://localhost:8070;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
        }

         location / {
               try_files $uri $uri/ /index.html;
         }

}
```

### Consumer app ssl (trace.unova.io)

```yaml
##
# You should look at the following URL's in order to grasp a solid understanding
# of Nginx configuration files in order to fully unleash the power of Nginx.
# https://www.nginx.com/resources/wiki/start/
# https://www.nginx.com/resources/wiki/start/topics/tutorials/config_pitfalls/
# https://wiki.debian.org/Nginx/DirectoryStructure
#
# In most cases, administrators will remove this file from sites-enabled/ and
# leave it as reference inside of sites-available where it will continue to be
# updated by the nginx packaging team.
#
# This file will automatically load configuration files provided by other
# applications, such as Drupal or Wordpress. These applications will be made
# available underneath a path with that package name, such as /drupal8.
#
# Please see /usr/share/doc/nginx-doc/examples/ for more detailed examples.
##

# Default server configuration
#
server {
        listen 80 default_server;
        listen [::]:80 default_server;
		

        # SSL configuration
        #
        # listen 443 ssl default_server;
        # listen [::]:443 ssl default_server;
        #
        # Note: You should disable gzip for SSL traffic.
        # See: https://bugs.debian.org/773332
        #
        # Read up on ssl_ciphers to ensure a secure configuration.
        # See: https://bugs.debian.org/765782
        #
        # Self signed certs generated by the ssl-cert package
        # Don't use them in a production server!
        #
        # include snippets/snakeoil.conf;

        root /var/www/html;

        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html;

				

        server_name trace.unova.io;
  # force redirect from http to https
				return 301 https://$host$request_uri;
		

        location /v1/ {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                #try_files $uri $uri/ =404;
                proxy_pass http://localhost:9100;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
        }

        location / {
               try_files $uri $uri/ /index.html;
         }

        # pass PHP scripts to FastCGI server
        #
        #location ~ \.php$ {
        #       include snippets/fastcgi-php.conf;
        #
}

server {
        listen 443 ssl;

       server_name trace.unova.io;
        #return 301 https://trace.unova.io$request_uri;
        root /var/www/html;

        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html;
        ssl_certificate /etc/nginx/ssl/unova_io.crt;
        ssl_certificate_key /etc/nginx/ssl/unova_io.key;

         location /v1/ {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                #try_files $uri $uri/ =404;
                proxy_pass http://localhost:9100;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
        }

         location / {
               try_files $uri $uri/ /index.html;
         }
			# reverse proxy http IP/url to an https 
				location /testnode/ {
				                proxy_pass http://207.154.204.185/;
				                #proxy_set_header Host $host;
				                proxy_http_version 1.1;
				                proxy_set_header Upgrade $http_upgrade;
				                proxy_set_header Connection 'upgrade';
				                proxy_set_header Host $host;
				                proxy_cache_bypass $http_upgrade;
				
				        }
# reverse proxy http IP/url to an https 
location /prodnode/ {
                proxy_pass http://68.183.87.157/;
                #proxy_set_header Host $host;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;

        }

}
                                                                                                                                                                                 60,5          Top
```