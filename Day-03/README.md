## Day-02

```
mkdir ~/backend && cd ~/backend
npm init -y
npm install express
nano index.js

const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello from Node.js backend!');
});

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});

- sudo unlink /etc/nginx/sites-enabled/default
- sudo nano /etc/nginx/sites-available/reverse-proxy
server {
    listen 80;
    server_name ip;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Host              $host;
        proxy_set_header X-Real-IP         $remote_addr;
        proxy_set_header X-Forwarded-For   $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Upgrade           $http_upgrade;
        proxy_set_header Connection        "upgrade";
    }
}

- sudo npm install -g npm@11.4.1
- pm2 start index.js
- pm2 stop indedx (if u wanna stop)

- sudo ln -s /etc/nginx/sites-available/reverse-proxy /etc/nginx/sites-enabled/
- sudo nginx -t
- sudo systemctl restart nginx
- *** remove 3000 from sg inbound and outbound ***
```