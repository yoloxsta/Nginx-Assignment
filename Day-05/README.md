## Day-05

### Loadbalancing With Nginx (On Single Instance)

```
- mkdir ~/load-balance-lab && cd ~/load-balance-lab
- cat app1.js

const http = require('http');
const port = 3001;

http.createServer((req, res) => {
  res.end('Hello from Backend 1 (Port 3001)');
}).listen(port, () => {
  console.log(`Backend 1 running on port ${port}`);
});

---

- cat app2.js

const http = require('http');
const port = 3002;

http.createServer((req, res) => {
  res.end('Hello from Backend 2 (Port 3002)');
}).listen(port, () => {
  console.log(`Backend 2 running on port ${port}`);
});

---
pm2 start app1.js
pm2 start app2.js

---
sudo nano /etc/nginx/sites-available/loadbalancer

upstream node_backends {
    server 127.0.0.1:3001;
    server 127.0.0.1:3002;
}

server {
    listen 80;
    server_name _;

    location / {
        proxy_pass http://node_backends;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}

---
- sudo ln -s /etc/nginx/sites-available/loadbalancer /etc/nginx/sites-enabled/
- sudo nginx -t
- sudo systemctl reload nginx

---
- SSL Termination

server {
#listen   80;
listen   443 ssl;
ssl_certificate    /etc/nginx/ssl/xyz.pem;
ssl_certificate_key    /etc/nginx/ssl/xyz.key;
  server_name abcdxyz;

  location ~ {
    proxy_pass_header Authorization;
    proxy_pass http://IP:port;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_http_version 1.1;
    proxy_set_header Connection "";
    proxy_buffering off;
    client_max_body_size 0;
    proxy_read_timeout 36000s;
    proxy_redirect off;
  }
}

```