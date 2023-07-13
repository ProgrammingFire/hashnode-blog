---
title: "Deploy .NET Minimal APIs To Kubernetes!"
seoDescription: "Kubernetes provides an easy way to scale your application, compared to virtual machines. It keeps code operational and speeds up the delivery process."
datePublished: Fri Mar 11 2022 11:05:31 GMT+0000 (Coordinated Universal Time)
cuid: cl0mbbv6001x3h8nv9xsw0v4t
slug: deploy-dotnet-minimal-apis-to-kubernetes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1650274074998/Dylacl7YK.png
tags: docker, kubernetes, apis, devops, dotnet

---

## Prerequisites

- .NET 6 SDK Has To Be Installed Locally To Start Developing The Server.
- Docker Should Be Installed. If You Are Using Windows Or Mac Install [Docker Desktop](https://www.docker.com/products/docker-desktop) To Get More Tools
- A Kubernetes Cluster Should Be Running On Cloud Or Local. If You Are Running Kubernetes On Local Computer Install [Minikube](https://minikube.sigs.k8s.io/docs/)

To check docker engine is working fine, run docker ps (If an error occurred docker is not running)

Kubectl version (If it shows both the client and server version you’re good to go)

## Building The .NET Minimal API

The First Thing We Need To Do Is To Create A Solution File By Running This Command: 
```bash
# Create A Blank Directory And Enter It
mkdir MinimalApi
cd MinimalApi

# Create A New Solution File
dotnet new sln
```

Now We Need To Create A New Minimal Web Api Project And Add That To Our Solution:
```bash
# Create A New Minimal Web Api Project
dotnet new web -o MinimalApi

# Add That To Our Solution
dotnet sln add MinimalApi
```

Now Our Project Is Done We Can Test It Now: 
```bash
# Run The MinimalApi Project
dotnet run --project MinimalApi
```

Now Navigate To [http://localhost:5160](http://localhost:5160) To See Your "Hello World" Minimal API

## Containerizing The Application With Docker

Because Kubernetes Is A Container Orchestration System. We Can Use Docker To Build These Containers

For That, We Need To Create A File Called `Dockerfile` That Should Define The Steps Needed To Run Our Application On the Production

```Dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["MinimalApi/MinimalApi.csproj", "MinimalApi/"]
RUN dotnet restore "MinimalApi/MinimalApi.csproj"
COPY . .
WORKDIR "/src/MinimalApi"
RUN dotnet build "MinimalApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MinimalApi.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MinimalApi.dll"]
```

### Run The Container Locally

```bash
# Build The Docker Image
docker build -t programmingfire/minimal-api:1.0.0 # Replace programmingfire With Your Username

# Run The Container With That Image
docker run -d -p 80:8000 --name minimal-api programmingfire/minimal-api:1.0.0
```

Now Navigate To [http://localhost:8000](http://localhost:8000) To See Your "Hello World" Minimal API

### Push The Container To Docker Hub

Create an Account On Docker Hub And Login With That Account With The Following Command

```bash
docker login
```

Now We Are Ready To Push This Image To Docker Hub

```bash
docker push programmingfire/minimal-api:1.0.0
```

## Deploy To Kubernetes

Create A Folder Called `Kubernetes` Inside Of The `MinimalApi` Folder

Inside Of `Kubernetes` Folder Create A File `Deployment.yml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: minimal-api-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: minimal-api
  template:
    metadata:
      labels:
        app: minimal-api
    spec:
      containers:
        - name: minimal-api
          image: programmingfire/minimal-api:1.0.0
          ports:
            - containerPort: 80
          imagePullPolicy: Always
```

### Create A Kubernetes Service

Inside Of `Kubernetes` Folder Create A File `LoadBalancer.yml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: minimal-api-service
spec:
  selector:
    app: minimal-api
  type: LoadBalancer
  ports:
    - port: 80
      targetPort: 80
```

### Apply Both Deployment And Service
```bash
# Apply Kubernetes Deployment
kubectl apply -f Kubernetes/Deployment.yml

# Apply Kubernetes Service
kubectl apply -f Kubernetes/LoadBalancer.yml
```

Now After Some Time When All The Kubernetes Pods Are Created And Up And Running You Can Test Your Application On [http://localhost](http://localhost) Again

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