## Create a Docker Image For Node.js

# Why Docker?

Docker allows you to package an application with its environment and all of its dependencies into a "box", called a container. Usually, a container consists of an application running in a stripped-to-basics version of a Linux operating system. An image is a blueprint for a container, a container is a running instance of an image.

# Create a Sample Node.js App (Optional)

In your terminal run:

```bash
# NPM
npm init
# YARN
yarn init
```

Then, create a `server.js` file that defines a web app using the [Express.js](https://expressjs.com) framework:

```js
const express = require("express");
// Constants
const PORT = 8080;
const HOST = "0.0.0.0";
// App
const app = express();
app.get("/", (req, res) => {
  res.send("Hello World");
});
app.listen(PORT, HOST);
console.log(`Running on http://${HOST}:${PORT}`);
```

# Creating Dockerfile

Create a file with the name of **Dockerfile**

The first thing we need to do is define from what image we want to build from. Here we will use the latest LTS (long term support) version 14 of node available from the [Docker Hub](https://hub.docker.com/_/node):

```dockerfile
FROM node:14
```

Next we create a directory to hold the application code inside the image, this will be the working directory for your application:

```dockerfile
# Create app directory
WORKDIR /usr/src/app
```

This image comes with Node.js and NPM already installed so the next thing we need to do is to install your app dependencies using the npm binary. Please note that if you are using npm version 4 or earlier a package-lock.json file will not be generated.

```dockerfile
# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./
RUN npm install
# If you are building your code for production
# RUN npm ci --only=production
```

Note that, rather than copying the entire working directory, we are only copying the **package.json** file. This allows us to take advantage of cached Docker layers. bitJudo has a good explanation of this [here](http://bitjudo.com/blog/2014/03/13/building-efficient-dockerfiles-node-dot-js/). Furthermore, the **npm ci** command, specified in the comments, helps provide faster, reliable, reproducible builds for production environments. You can read more about this [here](https://blog.npmjs.org/post/171556855892/introducing-npm-ci-for-faster-more-reliable).

To bundle your app's source code inside the Docker image, use the **COPY** instruction:

```dockerfile
# Bundle app source
COPY . .
```

Your app binds to port **8080** so you'll use the **EXPOSE** instruction to have it mapped by the **docker** daemon:

```dockerfile
EXPOSE 8080
```

Last but not least, define the command to run your app using **CMD** which defines your runtime. Here we will use **node server.js** to start your server:

```dockerfile
CMD [ "node", "server.js" ]
```

Your **Dockerfile** should now look like this:

```dockerfile
FROM node:14
# Create app directory
WORKDIR /usr/src/app
# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./
RUN npm install
# If you are building your code for production
# RUN npm ci --only=production
# Bundle app source
COPY . .
EXPOSE 8080
CMD [ "node", "server.js" ]
```

# Creating .dockerignore file

Create a **.dockerignore** file in the same directory as your **Dockerfile** with following content:

```ignore
node_modules
npm-debug.log
```

This will prevent your local modules and debug logs from being copied onto your Docker image and possibly overwriting modules installed within your image.

# Building The Image

Go to the directory that has your **Dockerfile** and run the following command to build the Docker image. The **-t** flag lets you tag your image so it's easier to find later using the **docker images** command:

```bash
docker build -t username/node-app-example .
```

# Running The Image

Running your image with **-d** runs the container in detached mode, leaving the container running in the background. The **-p** flag redirects a public port to a private port inside the container. Run the image you previously built:

```bash
docker run -p 8000:8080 -d username/node-app-example
```

Print the output of your app:

```bash
# Get container ID
$ docker ps
# Print app output
$ docker logs <container id>
# Example
Running on http://localhost:8080
```

If you need to go inside the container you can use the **exec** command:

```bash
# Enter the container
$ docker exec -it <container id> /bin/bash
```

## Now If You Open [http://localhost:8000/](http://localhost:8000/) You Should See "Hello World" In Your Browser