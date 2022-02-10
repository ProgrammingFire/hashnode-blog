## Deploy Node.js/Express Application On Ubuntu/Debian Server

# Step 1 - Download And Install Node.js

### Download Node.js From NodeSource

#### Let's download nodejs from Nodesource. NodeSource is a company which provides enterprise-grade Node support and maintains a repository containing the latest versions of Node.js.

```bash
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
```

### Install Node.js From Debian Repository

Let's install nodejs now:

```bash
sudo apt-get install nodejs -y
```

### Check The Version Of Node.js

```bash
node --version
```

### Check The Version Of NPM

```bash
npm --version
```

# Step 2 (Optional) - Create A Sample Node.js Project

#### Let's create a sample app and paste some basic code into it.

```bash
# Vim
vim app.js
# Nano
nano app.js
```

```js
// app.js
const express = require("express");
const app = express();
const port = 3000;
app.get("/", (req, res) => {
  res.send("Hello World!");
});
app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`);
});
```

### Lets now install express so that we can run this app server:

```bash
npm install express
```

### Run The Application

```bash
node app.js
```

### You should now be able to see the hello world page when you visit **http://your-server-ip:3000**

# Step 3 - Using pm2 As A Process Manager

#### Let's install and use pm2 as a process manager. Install pm2 using the commands below:

```bash
sudo npm i pm2 -g
```

#### Start the application using the following command:

```bash
pm2 start app.js
```

# Step 4 - Configuring Nginx As A Reverse Proxy

Now let's configure Nginx as a reverse proxy. This will help us get the security features from Nginx. Also, we can serve static content using Nginx.

#### Let's install Nginx using the following command:

```bash
sudo apt-get install nginx
```

#### Let's create a conf file for our Nodejs app using the command below

```bash
# Vim
sudo vim /etc/nginx/sites-available/myapp
# Nano
sudo nano /etc/nginx/sites-available/myapp
```

#### Copy the following content to this file, **Replace your-server-ip with your server ip**

```
server{
  server_name your-server-ip;
      location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### Activate this configuration using the command below:

```bash
sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled
```

## Visit http://your-server-ip/ and your application should work fine.