# Nginx

1. Serve web pages:
<blockquote>
  server {
    listen 80;
    server_name example.com;
    root /var/www/example.com;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
  }
 </blockquote>

