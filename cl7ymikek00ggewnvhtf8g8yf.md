---
title: "Explore the new Era of C++, The Carbon Language"
seoDescription: "Carbon is an open-source, statically-typed, compiled programming language initially built by Google to succeed in C++. Carbon offers modern programming..."
datePublished: Mon Sep 12 2022 10:29:50 GMT+0000 (Coordinated Universal Time)
cuid: cl7ymikek00ggewnvhtf8g8yf
slug: explore-the-carbon-language
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1662978432311/szp8swv0M.png
tags: cpp, introduction, google, programming-languages, carbon

---

## Introduction
C++, the successor to the C programming language, is unarguably one of the most relevant languages of the modern day. C++ powers Python machine learning, JavaScript libraries, game development, and many other tools used in modern programming languages.

For every Grand Challenge, if you get a solution, you’ll find some C++ somewhere on the bottom where you can’t see it. – Bjarne Stroustrup

C++ is also one of the older object-oriented programming languages with an adaptable design, versatility, and wide range of compatibility. C++ has inspired newer languages like Java, making the language viable for developing games, medical devices, AI and control systems, and many other applications.

Many modern programming languages like Rust and Golang have sprung up and allowed developers to write clean, safe code flexibly and with fewer hassles than their predecessors. However, older programming languages still power many codebases that run in our daily lives.

We’ve also seen programming languages roll out that proffer solutions to older ones like C/C++. In today’s world, they’re JavaScript, TypeScript, Objective-C, Swift, Java, and Kotlin.

In the recently concluded CPP North conference, Google announced that they’ve worked on a successor for C++: the Carbon programming language. In this article, we’ll go over what Carbon is, its features, how it differs from C++, how to set it up, and more.

Let’s get started!

## What is Carbon?
Within C++, there is a much smaller and cleaner language struggling to get out. – Bjarne Stroustrup

Carbon is an open-source, statically-typed, compiled programming language initially built by Google to succeed in C++. Carbon offers developers modern programming practices, such as generics, modular code organization, and simple syntax.

Carbon hopes to match C++’s performance and scalability. The language is designed for bidirectional interoperability with C++ for migration and adoption. This means you can transpile C++ codebases to Carbon and vice versa, as well as use existing C++ libraries.

Carbon is also very friendly and has a gentle learning curve for C++ developers, offering more expressivity and support for existing software design and architecture.

Carbon is still an experiment and is in its development phase. With that said, many features to help you write the Carbon code you’ll love haven’t been added yet.

## Hello, World!

Carbon’s syntax is actually quite similar to the syntax of Rust and a few other languages. Let’s take a look at an overview of Carbon’s syntax by understanding a Hello World program

```rust
package Sample api;

fn Main() -> i32 {
   Print("Hello, World");
   return 0;
}
```

## Interoperability
Logically, a successor language like Carbon needs to be compatible with its preceding language. Carbon is bidirectionally (two-way) compatible with C++, and you can use any of the languages with the others. Let’s take a look at its interoperability in action:
```c++
// Source code by the authors of the Carbon programming language

// C++ code used in both Carbon and C++:
struct Circle {
  float r;
};
```
```rust
package Geometry api;
import Cpp library "circle.h";
import Math;

fn PrintTotalArea(circles: Slice(Cpp.Circle)) {
  var area: f32 = 0;
  for (c: Cpp.Circle in circles) {
    area += Math.Pi * c.r * c.r;
  }
  Print("Total area: {0}", area);
}
```
In the example above, the Carbon code uses the circle.h C++ library to print the area of a circle.

## The modern generics system
Generics is one of the many great features you’ll find in modern programming languages, including Rust and Go. Carbon features a modern generics system with checked definitions and opt-in templates for seamless interoperability with existing C++ code.

Carbon’s generic system is one to look out for. It’ll ensure type checks for generic definitions to avoid the cost of definition checks at compile time. It’ll also ensure robust checked interfaces to reduce accidental dependencies on implementation and create significant explicit contracts.



## Installing Carbon
Carbon is still in an experimental mode so the language is far from production-ready. However, you can still play around with the current state of the language and consider contributing to its growth!

You’ll first need to have Homebrew installed on your computer. You can check out these installation instructions to install Homebrew on Linux and macOS.

Start by installing Bazelisk with the Homebrew package manager:

brew install bazelisk
Next, install `llvm` and export the PATH variable:

```bash
brew install llvm
export PATH="$(brew --prefix llvm)/bin:${PATH}"
```

Clone the Carbon repository and move into its directory on your machine:
```bash
git clone https://github.com/carbon-language/carbon-lang
cd carbon-lang
```
Finally, build and run the Carbon explorer:
```bash
bazel run //explorer -- ./explorer/testdata/print/format_only.carbon
And that’s it! Now you can begin experimenting with Carbon and checking out how the language works.
```

## Looking into Carbon’s future
Google and Carbon’s teams envision Carbon as a fast, scalable language that evolves and supports performance-critical software running on modern operating systems, hardware, and environments. Carbon would also sail towards backward or forward compatibility and a stable application binary interface (ABI) for the language.

An experimental but working version of Carbon is slated to be public by the end of 2022, and a full release will be available by 2024–2025.

## Conclusion
In this article, we covered an overview of Carbon and its features, compared Carbon to its predecessor, C++, and how its syntax works, as well as looked into where Carbon is hoping to go in the future.

Carbon is taking a batteries-included approach to developing the language. The repository is live on GitHub, where you can contribute to the discussion and development towards building a C++ successor everyone would love.