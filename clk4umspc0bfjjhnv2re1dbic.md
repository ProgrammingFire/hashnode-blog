---
title: "Create a Blazing Fast Backend with Rust and Rocket"
seoDescription: "Building a high-performance backend is crucial for modern web applications, where speed, scalability, and reliability are essential. Rust, a systems program"
datePublished: Sun Jul 16 2023 03:00:09 GMT+0000 (Coordinated Universal Time)
cuid: clk4umspc0bfjjhnv2re1dbic
slug: create-a-blazing-fast-backend-with-rust-and-rocket
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689436258567/a1c7806b-fdb3-45db-a841-ab05b7251e7c.png
tags: programming-blogs, web-development, backend, devops, rust

---

## Introduction

Building a high-performance backend is crucial for modern web applications, where speed, scalability, and reliability are essential. Rust, a systems programming language known for its safety, concurrency, and performance, is an excellent choice for creating such backends. When combined with the Rocket web framework, Rust becomes a powerful tool for building blazing-fast APIs. In this article, we'll explore how to create a fast backend using Rust and Rocket, and we'll go a step further by implementing a CRUD API.

## Prerequisites

Before we begin, ensure you have the following installed:

1. Rust: Install Rust by following the instructions on the official website ([https://www.rust-lang.org/tools/install](https://www.rust-lang.org/tools/install)).
    
2. Cargo: Cargo is Rust's package manager and builds system, and it comes bundled with the Rust installation.
    

## Step 1: Set Up a New Rust Project

To get started, open your terminal or command prompt and create a new Rust project using Cargo. Run the following command:

```bash
cargo new my-backend
```

This command creates a new directory named `my-backend` with the necessary project structure and files.

Navigate into the newly created project directory:

```bash
cd my-backend
```

## Step 2: Add Rocket as a Dependency

To use Rocket in our project, we need to add it as a dependency. Open the `Cargo.toml` file in your favorite text editor and add the following line under the `[dependencies]` section:

```bash
rocket = "0.5.0-rc.3"
```

Save the file and exit the editor. This configures our project to use the latest release candidate version of Rocket.

## Step 3: Create a Basic Rocket Application

Next, let's create a basic Rocket application. Open the `src/main.rs` file and replace its contents with the following code:

```rust
#[macro_use]
extern crate rocket;

use rocket::{serde::json::Json, State};
use rocket::fairing::AdHoc;
use rocket::tokio::sync::RwLock;
use serde::{Deserialize, Serialize};

#[derive(Debug, Deserialize, Serialize)]
struct Todo {
    id: u64,
    title: String,
    completed: bool,
}

#[derive(Default)]
struct AppState {
    todos: RwLock<Vec<Todo>>,
}

#[get("/")]
fn index() -> &'static str {
    "Hello, world!"
}

#[launch]
fn rocket() -> _ {
    let app_state = AppState::default();
    rocket::build()
        .attach(AdHoc::on_ignite("Todo API", |rocket| {
            Ok(rocket.manage(app_state))
        }))
        .mount("/", routes![index])
}
```

In this code, we define a basic Todo struct to represent our data model. We also define an `AppState` struct to manage the shared state of our application, which includes a `todos` vector wrapped in a read-write lock. Then we create a simple `index` route and add that to the `routes`.

### Get Todos

```rust
#[get("/todos")]
fn get_todos(state: &State<AppState>) -> Json<Vec<Todo>> {
    let todos = state.todos.read().expect("Failed to read todos lock");
    Json(todos.clone())
}
```

The `get_todos` function is a route handler that handles GET requests to the `/todos` endpoint. It reads the todos vector from the shared state and returns a JSON response containing the todos.

### Create Todo

```rust
#[post("/todos", data = "<todo>")]
fn create_todo(state: &State<AppState>, todo: Json<Todo>) -> Json<Todo> {
    let mut todos = state.todos.write().expect("Failed to acquire todos lock");
    let new_todo = todo.into_inner();
    todos.push(new_todo.clone());
    Json(new_todo)
}
```

The `create_todo` function is a route handler that handles POST requests to the `/todos` endpoint. It receives a JSON payload representing a new todo, adds it to the todos vector in the shared state, and returns the newly created todo as a JSON response.

### Update Todo

```rust
#[put("/todos/<id>", data = "<todo>")]
fn update_todo(state: &State<AppState>, id: u64, todo: Json<Todo>) -> Option<Json<Todo>> {
    let mut todos = state.todos.write().expect("Failed to acquire todos lock");
    for t in todos.iter_mut() {
        if t.id == id {
            *t = todo.into_inner();
            return Some(Json(t.clone()));
        }
    }
    None
}
```

The `update_todo` function is a route handler that handles PUT requests to the `/todos/<id>` endpoint. It receives a JSON payload representing an updated todo and searches for the todo with the provided id in the `todos` vector. If found, it updates the todo and returns it as a JSON response. Otherwise, it returns `None`.

### Delete Todo

```rust
#[delete("/todos/<id>")]
fn delete_todo(state: &State<AppState>, id: u64) -> Option<Json<Todo>> {
    let mut todos = state.todos.write().expect("Failed to acquire todos lock");
    let pos = todos.iter().position(|t| t.id == id)?;
    Some(Json(todos.remove(pos)))
}
```

The `delete_todo` function is a route handler that handles DELETE requests to the `/todos/<id>` endpoint. It searches for the todo with the provided id in the todos vector and removes it. If found, it returns the deleted todo as a JSON response. Otherwise, it returns `None`.

### Mount the Routes

Now we can mount these CRUD functions to the Routes like this:

```rust
rocket::build()
        .attach(AdHoc::on_ignite("Todo API", |rocket| {
            Ok(rocket.manage(app_state))
        }))
        .mount("/", routes![get_todos, create_todo, update_todo, delete_todo, index])
```

### Final App

The final `src/main.rs` looks like this:

```rust
#[macro_use]
extern crate rocket;

use rocket::{serde::json::Json, State};
use rocket::fairing::AdHoc;
use rocket::tokio::sync::RwLock;
use serde::{Deserialize, Serialize};

#[derive(Debug, Deserialize, Serialize)]
struct Todo {
    id: u64,
    title: String,
    completed: bool,
}

#[derive(Default)]
struct AppState {
    todos: RwLock<Vec<Todo>>,
}

#[get("/")]
fn index() -> &'static str {
    "Hello, world!"
}

#[get("/todos")]
fn get_todos(state: &State<AppState>) -> Json<Vec<Todo>> {
    let todos = state.todos.read().expect("Failed to read todos lock");
    Json(todos.clone())
}

#[post("/todos", data = "<todo>")]
fn create_todo(state: &State<AppState>, todo: Json<Todo>) -> Json<Todo> {
    let mut todos = state.todos.write().expect("Failed to acquire todos lock");
    let new_todo = todo.into_inner();
    todos.push(new_todo.clone());
    Json(new_todo)
}

#[put("/todos/<id>", data = "<todo>")]
fn update_todo(state: &State<AppState>, id: u64, todo: Json<Todo>) -> Option<Json<Todo>> {
    let mut todos = state.todos.write().expect("Failed to acquire todos lock");
    for t in todos.iter_mut() {
        if t.id == id {
            *t = todo.into_inner();
            return Some(Json(t.clone()));
        }
    }
    None
}

#[delete("/todos/<id>")]
fn delete_todo(state: &State<AppState>, id: u64) -> Option<Json<Todo>> {
    let mut todos = state.todos.write().expect("Failed to acquire todos lock");
    let pos = todos.iter().position(|t| t.id == id)?;
    Some(Json(todos.remove(pos)))
}

#[launch]
fn rocket() -> _ {
    let app_state = AppState::default();
    rocket::build()
        .attach(AdHoc::on_ignite("Todo API", |rocket| {
            Ok(rocket.manage(app_state))
        }))
        .mount("/", routes![get_todos, create_todo, update_todo, delete_todo])
}
```

## Step 4: Build and Run the Backend

Now, let's build and run our backend server. In the terminal, run the following command:

```bash
cargo run
```

Cargo will compile the project and start the Rocket server. You should see output similar to the following:

```bash
🚀 Rocket has launched from http://localhost:8000
```

Congratulations! Your Rocket server is now running.

## Step 5: Test the CRUD API

To test our CRUD API, we can use cURL or any API testing tool like Postman. Here are some example API requests you can try:

1. **Create a Todo**:
    

* ```bash
    curl -X POST -H "Content-Type: application/json" -d '{"id": 1, "title": "Buy groceries", "completed": false}' http://localhost:8000/todos
    ```
    
* **Get All Todos**:
    
* ```bash
    curl http://localhost:8000/todos
    ```
    
* **Update a Todo**:
    
* ```bash
    curl -X PUT -H "Content-Type: application/json" -d '{"id": 1, "title": "Buy groceries", "completed": true}' http://localhost:8000/todos/1
    ```
    
* **Delete a Todo**:
    

1. ```bash
    curl -X DELETE http://localhost:8000/todos/1
    ```
    

Feel free to explore the API further and add more endpoints and functionalities as per your requirements.

## Conclusion

In this article, we explored how to create a blazing-fast backend using Rust and the Rocket web framework. We learned how to set up a new Rust project, add Rocket as a dependency, and create a basic Rocket application. We went a step further by implementing a CRUD API using Rocket's powerful routing and request-handling capabilities.

By leveraging the performance and safety features of Rust and the expressive syntax of Rocket, you can build high-performance APIs that are robust, efficient, and easily maintainable. Rust's strict memory and concurrency model ensures memory safety and eliminates data races, while Rocket simplifies the process of building RESTful APIs with its intuitive syntax and powerful features.

With Rust and Rocket, you have a powerful combination for building high-performance backends that can handle heavy workloads while providing fast response times. So go ahead, dive into the world of Rust and Rocket, and start building blazing-fast backends for your next project!