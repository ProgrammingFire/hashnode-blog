---
title: "picoCTF 2022: Reverse Engineering: file-run-1"
seoDescription: "A program has been provided to you, what happens if you try to run it on the command line? Download the program here. We have been provided with a file..."
datePublished: Sun Oct 16 2022 12:27:06 GMT+0000 (Coordinated Universal Time)
cuid: cl9bboco5000m09l63zwy1qjc
slug: picoctf-2022-reverse-engineering-file-run-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1665923137924/jrxeSSBAp.png
tags: reverse-engineering, cyber-security, ctf, picoctf, picoctf-2022

---

## Introduction
**Challenge:** [file-run-1](https://play.picoctf.org/practice/challenge/266)

**Category:** Reverse Engineering

**Description:**

A program has been provided to you, what happens if you try to run it on the command line? Download the program [here](https://artifacts.picoctf.net/c/310/run).

## Solution

We have been provided with a file called `run`. If you try to run the file like this:
```bash
./run
```
It won't work as expected. You should first give yourself permission to execute this file.
```bash
chmod +x ./run
```
Now if you try to run this, it will work. Just like this:
```bash
./run
picoCTF{U51N6_Y0Ur_F1r57_F113_e5559d46}
```

## Conclusion
This one is what I think is the easiest challenge ever on the picoCTF 2022.

**Flag:** `picoCTF{U51N6_Y0Ur_F1r57_F113_e5559d46}`
