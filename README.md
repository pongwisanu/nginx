# Nginx

1. Serve web pages:

```
server {
    listen 80;
    server_name example.com;
    root /var/www/example.com;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

2. Load balancing:

```
upstream backend {
    server backend1.example.com;
    server backend2.example.com;
    server backend3.example.com;
}

server {
    listen 80;
    server_name example.com;
    location / {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

3. Reverse proxy:

```
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend.example.com;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

4. SSL/TLS termination:

```
server {
    listen 443 ssl;
    server_name example.com;
    root /var/www/example.com;
    index index.html index.htm;

    ssl_certificate /path/to/cert.pem;
    ssl_certificate_key /path/to/key.pem;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

5. Caching:

```
proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=my_cache:10m;

server {
    listen 80;
    server_name example.com;

    location / {
        proxy_cache my_cache;
        proxy_pass http://backend.example.com;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

6. HTTP compression:

```
gzip on;
gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss;

server {
    listen 80;
    server_name example.com;
    root /var/www/example.com;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

7. Rate limiting:

```
limit_req_zone $binary_remote_addr zone=my_zone:10m rate=1r/s;

server {
    listen 80;
    server_name example.com;

    location / {
        limit_req zone=my_zone burst=5;
        proxy_pass http://backend.example.com;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

8. Authentication and authorization:

```
auth_basic "Restricted Content";
auth_basic_user_file /etc/nginx/.htpasswd;

server {
    listen 80;
    server_name example.com;
    root /var/www/example.com;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
        auth_basic "Restricted Content";
        auth_basic_user_file /etc/nginx/.htpasswd;
    }
}
```

9. Websockets:

```
map $http_upgrade $connection_upgrade {
    default upgrade;
    '' close;
}

server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend.example.com;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
    }
}
```
