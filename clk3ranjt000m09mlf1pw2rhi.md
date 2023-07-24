---
title: "Deploy Go Minimal API to Docker and Kubernetes"
seoDescription: "Go Lang is a powerful programming language known for its simplicity, performance, and strong concurrency support. With the rise of containerization and ..."
datePublished: Sat Jul 15 2023 08:38:57 GMT+0000 (Coordinated Universal Time)
cuid: clk3ranjt000m09mlf1pw2rhi
slug: deploy-go-minimal-api-to-docker-and-kubernetes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690194465300/27bd0170-7b24-4742-a22a-755178085963.png
tags: docker, go, kubernetes, apis, devops

---

## Introduction

Go Lang is a powerful programming language known for its simplicity, performance, and strong concurrency support. With the rise of containerization and orchestration technologies like Docker and Kubernetes, deploying Go applications has become even more convenient and scalable. In this article, we will explore how to deploy a Go Lang minimal API to Docker and Kubernetes, enabling you to easily containerize and scale your Go applications.

## Prerequisites

Before we begin, make sure you have the following installed:

1. Go Lang: You can download and install Go from the official website ([https://golang.org](https://golang.org)).
    
2. Docker: Install Docker from the official website ([https://www.docker.com](https://www.docker.com)) for your operating system.
    
3. Kubernetes: If you want to deploy to a Kubernetes cluster, make sure you have a running Kubernetes cluster set up or use a cloud-based Kubernetes service like Google Kubernetes Engine (GKE) or Amazon Elastic Kubernetes Service (EKS).
    

## Step 1: Create a Minimal Go API

First, let's create a minimal Go API. Open your preferred text editor and create a new file called `main.go`. Add the following code to create a simple HTTP server that listens on port 8080 and responds with a "Hello, World!" message:

```go
package main

import (
	"fmt"
	"net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hello, World!")
}

func main() {
	http.HandleFunc("/", handler)
	http.ListenAndServe(":8080", nil)
}
```

Save the file and exit the editor. We now have a minimal Go API that we can deploy to Docker and Kubernetes.

## Step 2: Dockerize the Go API

To containerize our Go API, we'll create a Dockerfile that specifies the build steps and runtime environment. Create a new file called `Dockerfile` in the same directory as `main.go` and add the following code:

```dockerfile
# Use the official Go image as the base image
FROM golang:1.16-alpine

# Set the working directory inside the container
WORKDIR /app

# Copy the Go modules files
COPY go.mod .
COPY go.sum .

# Download and install Go dependencies
RUN go mod download

# Copy the source code into the container
COPY . .

# Build the Go application
RUN go build -o main .

# Expose the port the application listens on
EXPOSE 8080

# Set the entry point of the container
CMD ["./main"]
```

Save the Dockerfile and exit the editor. We've defined a multi-stage Docker build that downloads Go dependencies, copies the source code, builds the Go application, and sets the entry point to run our API.

## Step 3: Build and Run the Docker Image

Next, let's build and run the Docker image locally. Open a terminal or command prompt in the same directory as `main.go` and `Dockerfile`. Run the following command to build the Docker image:

```bash
docker build -t my-go-api .
```

The `-t` flag sets the name and tag for the Docker image (you can choose any name you like).

Once the build is complete, run the following command to start a Docker container using the image we just built:

```bash
docker run -p 8080:8080 my-go-api
```

The `-p` flag maps the host port 8080 to the container port 8080. Now, if you open your browser and navigate to [`http://localhost:8080`](http://localhost:8080), you should see the "Hello, World!" message.

## Step 4: Deploy to Kubernetes

If you have a Kubernetes cluster set up, you can deploy our Go API as a Kubernetes deployment. Create a new file called `deployment.yaml` and add the following Kubernetes deployment configuration:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: go-api-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: go-api
  template:
    metadata:
      labels:
        app: go-api
    spec:
      containers:
        - name: go-api
          image: my-go-api
          ports:
            - containerPort: 8080
```

Save the `deployment.yaml` file. This configuration specifies a deployment with three replicas and exposes port 8080. It uses the Docker image we built earlier.

To deploy the application to Kubernetes, run the following command:

```bash
kubectl apply -f deployment.yaml
```

Kubernetes will create the deployment and manage the desired number of replicas of our Go API.

## Conclusion

In this article, we learned how to deploy a minimal Go Lang API to Docker and Kubernetes. We started by creating a simple Go API, then containerized it using Docker and created a Docker image. We also explored how to build and run the Docker image locally. Finally, we deployed the application to a Kubernetes cluster using a deployment configuration.

Now you know how to take your Go applications and deploy them to containers, enabling scalability and portability across different environments. Containerization with Docker and orchestration with Kubernetes provide a powerful infrastructure for deploying and managing Go applications at scale.