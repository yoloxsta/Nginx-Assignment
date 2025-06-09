## Day-02 
### server block with Nginx

```
- create a folder under /var/www/
(default is html that have indedx.html)

- create a server block config under /etc/nginx/sites-available
-  cat helloworld
server {
    listen 80;
    server_name IP;

    root /var/www/helloworld;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

- sudo ln -s /etc/nginx/sites-available/helloworld /etc/nginx/sites-enabled/
- Then, you'll see "helloworld -> /etc/nginx/sites-available/helloworld" under /etc/nginx/sites-enabled


```