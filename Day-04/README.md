### Day-04

#### on-off by Nginx (Toggle Method)
```
cat hello.conf
server {
    listen 80;
    server_name ip;
    location / {
        return 200 "Hello, Nginx is working!\n";
        add_header Content-Type text/plain;
    }
}

---
cat hello.conf
server {
    listen 80;
    server_name ip;
    location / {
        return 200 "Hello, Nginx is working!\n";
        add_header Content-Type text/plain;
    }
}

---
- cat prod-nginx

#!/bin/bash

# Clear all enabled configs first
sudo rm -f /etc/nginx/sites-enabled/*

case "$1" in
    "on")
        sudo ln -s /etc/nginx/sites-available/hello.conf /etc/nginx/sites-enabled/
        echo "Enabled hello.conf (200 OK)"
        ;;
    "off")
        sudo ln -s /etc/nginx/sites-available/hello-403.conf /etc/nginx/sites-enabled/
        echo "Enabled hello-403.conf (403 Forbidden)"
        ;;
    *)
        echo "Usage: sudo toggle-nginx [on|off]"
        exit 1
        ;;
esac

sudo systemctl restart nginx

--- 
- sudo chmod +x filename (for executable)
---
 run >

 - sudo filename on (Ui enable by nginx)
 - sudo filename off (Ui disable by nginx)

```