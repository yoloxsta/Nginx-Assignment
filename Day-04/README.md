### Day-04

#### ON-OFF By Nginx (Toggle Method)
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
cat hello-403.conf

server {
    listen 80;
    server_name ip;
    return 403;
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


---
- for nacos
#!/bin/bash

CONF_DIR="/etc/nginx/conf.d"
CONF_FILE="nacos.conf"
CONF_403_FILE="nacos-403.conf"

function enable_nacos() {
    cd "$CONF_DIR" || exit 1
    mv "${CONF_FILE}.off" "$CONF_FILE" 2>/dev/null || { echo "Error: ${CONF_FILE}.off not found"; exit 1; }
    mv "$CONF_403_FILE" "${CONF_403_FILE}.off" 2>/dev/null
    systemctl restart nginx && echo "Enabled ${CONF_FILE} (200 OK)"
}

function disable_nacos() {
    cd "$CONF_DIR" || exit 1
    mv "$CONF_FILE" "${CONF_FILE}.off" 2>/dev/null || { echo "Error: ${CONF_FILE} not found"; exit 1; }
    mv "${CONF_403_FILE}.off" "$CONF_403_FILE" 2>/dev/null
    systemctl restart nginx && echo "Enabled ${CONF_403_FILE} (403 Forbidden)"
}

case "$1" in
    "on")  enable_nacos ;;
    "off") disable_nacos ;;
    *)     echo "Usage: $0 [on|off]"; exit 1 ;;
esac

---

```
##