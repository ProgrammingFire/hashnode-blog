---
title: "Setup A Django Application"
seoDescription: "Django is a high-level Python web framework that encourages rapid development and clean, pragmatic design. Built by experienced developers, it takes care..."
datePublished: Wed Dec 22 2021 10:36:45 GMT+0000 (Coordinated Universal Time)
cuid: ckxheil2500huhts1265k3ebp
slug: setup-django-application
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1650273846193/p9oRUCWj2.png
tags: python, django, applications, setup

---

# Step 1 - Install Python

You need to have Python before installing Django

## Windows

Go to [python.org](https://python.org/download) and download the executable run through installation

## Linux

In linux chances are that you already get python installed but if not install for your distribution

### Debian/Ubuntu

```bash
sudo apt-get install python3
```

### Arch/Manjaro

```bash
sudo pacman -S python
```

### Fedora/Redhat

```bash
sudo dnf install python3
```

### Gentoo

```bash
sudo emerge -a python
```

## Mac OS

In mac os you already get python pre installed

# Step 2 - Setup Virtual Environment

We can create a virtual env so we can keep packages seperate for our project

#### Install Virtual Env

```bash
pip install virtualenv
```

#### Create a VirtualEnv

```bash
virtualenv venv
```

#### Activate VirtualEnv

```bash
source venv/bin/activate
```

# Step 3 - Install Django

To Install Django Run The Following Command:

```bash
pip install django
```

# Step 4 - Create New Django Project

To create a new django project run the following command:

```bash
django-admin startproject my-project
```

# Step 5 - Create Admin User

To create a admin user run the following command

```bash
python manage.py createsuperuser
```

# Run The App !

```bash
python manage.py runserver
```