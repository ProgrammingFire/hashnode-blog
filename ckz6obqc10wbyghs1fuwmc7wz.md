---
title: "Deploy Node.JS Application To Kubernetes!"
seoDescription: "Kubernetes provides an easy way to scale your application, compared to virtual machines. It keeps code operational and speeds up the delivery process. Kuber"
datePublished: Thu Feb 03 2022 07:45:18 GMT+0000 (Coordinated Universal Time)
cuid: ckz6obqc10wbyghs1fuwmc7wz
slug: deploy-nodejs-application-to-kubernetes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1650274018809/ZppHVTxuT.png
tags: docker, javascript, nodejs, kubernetes, devops

---

## Prerequisites

- NodeJS Has To Be Installed Locally To Start Developing The Server.
- Docker Should Be Installed. If You Are Using Windows Or Mac Install [Docker Desktop](https://www.docker.com/products/docker-desktop) To Get More Tools
- A Kubernetes Cluster Should Be Running On Cloud Or Local. If You Are Running Kubernetes On Local Computer Install [Minikube](https://minikube.sigs.k8s.io/docs/)

To check docker engine is working fine, run docker ps (If an error occurred docker is not running)

Kubectl version (If it shows both the client and server version you’re good to go)

## Building The Node Application

You Can Freely Use Your Own Application Here But I Am Going To Clone A Starter Repo From GitHub Which Uses TypeScript, Node.JS, Express For The Tech Stack

```bash
git clone https://github.com/programmingfire/node-express-typescript-starter.git
```

When The Repo Is Cloned Go-Ahead Run The Following Commands To Build And Start The Application

```bash
# Installs All The Dependencies
npm install

# Compiles The TypeScript Code
npm run build

# Start The Application
npm run start
```

Now Open Your Browser And Navigate To [http://localhost:4000](http://localhost:4000) And You Should See Hello World From The Server Also Go To [http://localhost:4000/api](http://localhost:4000/api) To See The Response In JSON

Now Our Application Is Successfully Running

## Containerizing The Application With Docker

Because Kubernetes Is A Container Orchestration System. We Can Use Docker To Build These Containers

For That, We Need To Create A File Called `Dockerfile` That Should Define The Steps Needed To Run Our Application On the Production

%[https://gist.github.com/programmingfire/c8beb43d579d202b9586b29e0edf3430]

## Run The Container Locally

```bash
# Build The Docker Image
docker build -t programmingfire/node-app:1.0.0 # Replace programmingfire With Your Username

# Run The Container With That Image
docker run -d -p 4000:8000 --name node-app programmingfire/node-app:1.0.0
```

Now Open Your Browser And Navigate To [http://localhost:4000](http://localhost:4000) And You Should See Hello World From The Server Also Go To [http://localhost:4000/api](http://localhost:4000/api) To See The Response In JSON

### Push The Container To Docker Hub

Create an Account On Docker Hub And Login With That Account With The Following Command

```bash
docker login
```

Now We Are Ready To Push This Image To Docker Hub

```bash
docker push programmingfire/node-app:1.0.0
```

## Deploy To Kubernetes

Create A Folder Called `k8s`

### Create A Kubernetes Deployment

Inside Of `k8s` Folder Create A File `deployment.yml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: node-app
  template:
    metadata:
      labels:
        app: node-app
    spec:
      containers:
      - name: node-app
        image: programmingfire/node-app:1.0.0
        resources:
          limits:
            memory: "128mi"
            cpu: "500m"
        ports:
          - containerPort: 8000
        imagePullPolicy: Always
```

### Create A Kubernetes Service

Inside Of `k8s` Folder Create A File `service.yml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: node-app
spec:
  selector:
    app: node-app
  type: LoadBalancer
  ports:
  - port: 4000
    targetPort: 8000
```

### Apply Both Deployment And Service
```bash
# Apply Kubernetes Deployment
kubectl apply -f k8s/deployment.yml

# Apply Kubernetes Service
kubectl apply -f k8s/service.yml
```

Now After Some Time When All The Kubernetes Pods Are Created And Up And Running You Can Test Your Application On [http://localhost:4000](http://localhost:4000) Again

Just For Fun Let's Kill One Of The Pod In Kubernetes And See How Much Fast Is Kubernetes To Recover That

```bash
# List All The Pods
kubectl get pods

# Copy The ID Of Any Pod
kubectl delete pod <pod-id-here>
```

Now One Of 3 Pods Is Deleted

```bash
# List All The Pods Again
kubectl get pods 
```

All 3 Out Of 3 Pods Should Be Running Because Kubernetes ReCreated That Container When One Pod Get Deleted