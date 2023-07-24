---
title: "Why Docker is just pure magic 🛸🔥 how to work with it?"
seoDescription: "Docker has taken the world of software development and deployment by storm, revolutionizing the way applications are built, shipped, and run. It's no ..."
datePublished: Mon Jul 24 2023 15:29:47 GMT+0000 (Coordinated Universal Time)
cuid: clkh0xnlq000f09lj2f230d1s
slug: why-docker-is-just-pure-magic-how-to-work-with-it
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690212541363/43a718a4-3459-4ff0-8ee0-255d4fdfd2a4.png
tags: cloud, docker, kubernetes, cloud-computing, devops

---

## Introduction

Docker has taken the world of software development and deployment by storm, revolutionizing the way applications are built, shipped, and run. It's no exaggeration to say that Docker is pure magic! In this article, we'll explore the reasons why Docker is so amazing and dive into how you can work with it to streamline your development workflow and deploy applications like a pro, using a more complex GoLang application with a REST API example.

## The Magic of Docker

Docker introduces containerization, a technology that enables developers to package applications and all their dependencies into a single container. This container can then run consistently across any environment, ensuring that the application works seamlessly from development to production. Let's explore the magical aspects of Docker that make it so special:

### 1\. Lightweight and Portable

Docker containers are incredibly lightweight, as they share the host operating system's kernel, making them more efficient than traditional virtual machines. This means you can run multiple containers on a single host without worrying about resource overhead. Additionally, Docker containers are highly portable, allowing you to run the same containerized application on different environments without any modifications.

### 2\. Isolation and Consistency

Docker provides isolation between containers, which ensures that each application runs in its isolated environment, independent of other applications. This isolation prevents conflicts between dependencies and eliminates the notorious "it works on my machine" issue. With Docker, you can achieve consistency across various development, testing, and production environments, leading to more reliable applications.

### 3\. Rapid Application Deployment

One of the magical features of Docker is its ability to accelerate the application deployment process. With Docker, you can package your application and its dependencies into a container image. This image can then be deployed with a single command, making the deployment process faster and less error-prone.

### 4\. Version Control for Applications

Docker allows version control for container images, similar to how version control systems like Git work for code. Each change to a Dockerfile (the recipe for building a Docker image) or a base image can be versioned. This ensures that you can track changes, roll back to previous versions, and collaborate effectively with your team when building and deploying applications.

## Working With Docker - A GoLang REST API Application

Now that we've witnessed the magic of Docker, let's dive into how you can work with Docker using a more complex GoLang application as an example. We'll create a REST API using the popular Gorilla Mux package, containerize the application, build a Docker image, and run it in a container.

### Step 1: Setting Up the GoLang REST API

Let's create a more complex GoLang application that exposes a simple REST API. We'll use the Gorilla Mux package for routing. Create a file named `main.go` with the following code:

```go
// main.go
package main

import (
	"encoding/json"
	"log"
	"net/http"

	"github.com/gorilla/mux"
)

// Book struct representing a book
type Book struct {
	ID     string `json:"id"`
	Title  string `json:"title"`
	Author string `json:"author"`
}

// Global slice to store books
var books []Book

// GetBooksHandler returns all books in JSON format
func GetBooksHandler(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(books)
}

// AddBookHandler adds a new book to the books slice
func AddBookHandler(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("Content-Type", "application/json")
	var book Book
	_ = json.NewDecoder(r.Body).Decode(&book)
	books = append(books, book)
	json.NewEncoder(w).Encode(books)
}

func main() {
	// Sample data
	books = append(books, Book{ID: "1", Title: "The Go Programming Language", Author: "Alan A. A. Donovan"})
	books = append(books, Book{ID: "2", Title: "Effective Go", Author: "Rob Pike"})

	router := mux.NewRouter()

	// Define routes
	router.HandleFunc("/books", GetBooksHandler).Methods("GET")
	router.HandleFunc("/books", AddBookHandler).Methods("POST")

	log.Fatal(http.ListenAndServe(":8000", router))
}
```

### Step 2: Writing the Dockerfile

Next, we'll create a Dockerfile, which will instruct Docker on how to build the container image for our GoLang REST API application. Create a file named `Dockerfile` (no file extension) in the same directory as `main.go` with the following content:

```Dockerfile
# Use the official GoLang image as the base image
FROM golang:1.17-alpine

# Set the working directory inside the container
WORKDIR /app

# Copy the GoLang application code to the container
COPY main.go .

# Build the GoLang application inside the container
RUN go build -o myapp

# Expose the port the API will listen on
EXPOSE 8000

# Set the command to run the application
CMD ["./myapp"]
```

### Step 3: Building the Docker Image

With the Dockerfile in place, you can now build the Docker image for your GoLang REST API application. Open a terminal in the same directory as the Dockerfile and run the following command:

```bash
docker build -t my-golang-api .
```

The `-t` flag assigns a name (`my-golang-api`) to the image, allowing you to reference it easily.

### Step 4: Running the GoLang REST API in a Docker Container

With the Docker image built, you can now run the GoLang REST API in a Docker container. Run the following command:

```bash
docker run -p 8000:8000 my-golang-api
```

The `-p` flag maps port 8000 from the container to port 8000 on the host, allowing you to access the API from your local machine.

### Step 5: Interacting with the REST API

You can now interact with the GoLang REST API running inside the Docker container. You can use tools like cURL or Postman to make API requests. Here are some example API requests:

**Get all books:**

```bash
curl http://localhost:8000/books
```

**Add a new book:**

```bash
curl -X POST -H "Content-Type: application/json" -d '{"id": "3", "title": "Clean Code", "author": "Robert C. Martin"}' http://localhost:8000/books
```

You should see the list of books in JSON format when making a GET request and receive the updated list of books when adding a new book.

## Conclusion

In conclusion, Docker is pure magic in the world of software development. It brings the wonders of containerization, lightweight and portable applications, isolation, and rapid deployment to developers worldwide. Working with Docker is not only a delight but also an essential skill for modern developers.

By using Docker, you can simplify the development workflow, ensure consistency between environments, and speed up application deployment. It's a game-changer that empowers developers to create, ship, and run applications effortlessly.

So, if

you haven't already embarked on the magical journey of Docker, now is the time to do so. Install Docker, build your first Docker image for your GoLang REST API application, run containers, and experience the pure magic of Docker for yourself. Happy containerizing! 🚀🔮