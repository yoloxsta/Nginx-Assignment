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





```