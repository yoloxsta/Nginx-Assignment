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

- If you wanna setup default , sudo rm /etc/nginx/sites-enabled/helloworld

```
### Server Block Lab Recap

| Step  | Description                                                                     |
| ----- | ------------------------------------------------------------------------------- |
| **1** | Created a custom HTML page in `/var/www/helloworld/index.html`                  |
| **2** | Created a new Nginx config file `/etc/nginx/sites-available/helloworld`         |
| **3** | Added a `server` block that listens on port 80 and serves `/var/www/helloworld` |
| **4** | Enabled the server block using a symlink to `/etc/nginx/sites-enabled/`         |
| **5** | Reloaded Nginx (`sudo systemctl reload nginx`)                                  |
| **6** | Confirmed it works by visiting your EC2 public IP and seeing your custom page   |
| **7** | Learned how to manage server blocks alongside the default site                  |

### Add a Second Virtual Host to Nginx

```
sudo mkdir -p /var/www/secondsite
echo "<h1>Hello from Second Virtual Host!</h1>" | sudo tee /var/www/secondsite/index.html

sudo nano /etc/nginx/sites-available/secondsite

server {
    listen 8080;
    server_name IP;

    root /var/www/secondsite;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

- sudo ln -s /etc/nginx/sites-available/secondsite /etc/nginx/sites-enabled/
- sudo systemctl reload nginx
- call http://<your-EC2-IP>:8080/

```