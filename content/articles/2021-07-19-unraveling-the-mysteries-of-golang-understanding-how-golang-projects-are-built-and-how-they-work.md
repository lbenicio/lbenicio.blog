---

type: "posts"
title: 'Unraveling the Mysteries of Golang: Understanding How Golang Projects are
  Built and How They Work'
icon: fa-comment-alt
tags: golang go tutorial discussion
categories: ["tutorial"]

date: "2021-07-19"
type: posts
image: https://github.com/lbenicio/lbenicio.blog

image_alt: https://github.com/lbenicio/lbenicio.blog

---



Golang, also known as Go, is a popular programming language that has been gaining traction over the years. It was created in 2007 by Google's Robert Griesemer, Rob Pike, and Ken Thompson, with the aim of being a simple, fast, and efficient language that can handle modern software development needs. One of the unique features of Golang is its ability to handle concurrency with ease, making it ideal for building large-scale distributed systems. In this article, we'll dive into the details of Golang projects, how they are built, and how they work.

## Building Golang Projects

To build a Golang project, you need to have the Go programming language installed on your system. Once you have installed Go, you can create a new project by creating a new directory, navigating to it in your terminal, and running the command go mod init <module-name>. This will create a new module, which is a collection of Go packages that can be versioned and managed using Go modules.

Once you have created a new module, you can start creating Go files inside it. A Go file is a text file that contains Go code. Each file should have a package statement at the top, which declares which package the file belongs to. The main package is a special package that is used for executable programs. To build an executable program, you need to have a file named main.go inside your main package. This file should contain a main function, which is the entry point of your program.

To build your Golang project, you can run the command go build in your terminal. This will compile your project and generate an executable file that you can run. If you want to install your project, you can run the command go install. This will compile your project and install the resulting binary in your Go bin directory, which is usually located at $GOPATH/bin.

## How Golang Works

Golang is a compiled language, which means that your code is compiled into machine code that can be executed by the computer's processor. When you run a Golang program, the Go compiler first translates your source code into an intermediate format called Go Assembly. This intermediate code is then translated into machine code by the computer's processor.

One of the key features of Golang is its support for concurrency. Concurrency is the ability to run multiple tasks at the same time. Golang uses goroutines, which are lightweight threads that can be created and managed with ease. Goroutines are very efficient because they use very little memory and can be scheduled by the Golang runtime scheduler, which ensures that they are executed in a fair and efficient manner.

Golang also has a built-in garbage collector, which is responsible for managing memory allocation and deallocation. The garbage collector uses a technique called mark and sweep, which involves marking all objects that are still in use and then sweeping away all objects that are no longer in use.

## Conclusion

Golang is a powerful and efficient programming language that is ideal for building large-scale distributed systems. Its support for concurrency and its built-in garbage collector make it easy to write efficient and scalable code. Building a Golang project is straightforward, and it can be done using the Go command-line tools. Understanding how Golang projects are built and how they work is essential for any developer who wants to work with Golang. With its simplicity, speed, and efficiency, Golang is quickly becoming one of the most popular programming languages in the world.

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)