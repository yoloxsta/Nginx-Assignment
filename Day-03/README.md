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

### Reverse Proxy for Multiple Backend Services

```
mkdir ~/backend1
cd ~/backend1
npm init -y
npm install express

cat > index.js <<EOF
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello from Backend 1!');
});

app.listen(port, '127.0.0.1', () => {
  console.log(\`Backend1 running on http://localhost:\${port}\`);
});
EOF

---

mkdir ~/backend2
cd ~/backend2
npm init -y
npm install express

cat > index.js <<EOF
const express = require('express');
const app = express();
const port = 4000;

app.get('/', (req, res) => {
  res.send('Hello from Backend 2!');
});

app.listen(port, '127.0.0.1', () => {
  console.log(\`Backend2 running on http://localhost:\${port}\`);
});
EOF

--- 

- pm2 start ~/backend1/index.js --name backend1
- pm2 start ~/backend2/index.js --name backend2

- sudo nano /etc/nginx/sites-available/reverse-proxy
server {
    listen 80;
    server_name your-ec2-ip-or-domain;

    location /api/users/ {
        proxy_pass http://127.0.0.1:3000/;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /api/products/ {
        proxy_pass http://127.0.0.1:4000/;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

- sudo ln -s /etc/nginx/sites-available/reverse-proxy /etc/nginx/sites-enabled/
- sudo nginx -t  # Test configuration
- sudo systemctl reload nginx

Open a browser and visit:

http://your-ec2-ip/api/users/ → Should show: Hello from Backend 1!

http://your-ec2-ip/api/products/ → Should show: Hello from Backend 2!

```