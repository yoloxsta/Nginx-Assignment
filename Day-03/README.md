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



```