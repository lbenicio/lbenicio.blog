---

layout: posts
title: Podman as Orchestraitor
icon: fa-comment-alt
tag: tip, development, software, orchestraitor, containers
categories: development
toc: true
---


Podman is an open source tool for managing containers and container images. It is an alternative to Docker and is designed to be more secure, efficient, and portable. As a container orchestrator, Podman provides an easy and efficient way to develop, test, and deploy software applications in containers.

# Podman as Orchestraitor

One of the main benefits of using Podman for software development is that it allows you to run containers without requiring a daemon. This means that you can run containers directly on your host system, without the need for an additional service to manage them. This can be particularly useful for developers who want to test their applications in a more realistic environment, as it allows them to run their containers in the same environment as their host system.

In addition to running containers directly on your host system, Podman also provides a number of other useful features for software development. For example, it allows you to easily build and manage container images, as well as to run and manage multiple containers at once. This can be particularly useful when developing applications that require multiple containers to work together, as you can easily coordinate the different containers using Podman.

# Examples

To use Podman for software development, you will need to install it on your host system. This is typically done using a package manager, such as yum or dnf on Fedora, or apt-get on Ubuntu. Once you have installed Podman, you can use it to manage your containers and container images using the command line.

Here is a simple example of how you can use Podman to run a container:

```bash
$ podman run -it ubuntu:20.04 bash
```

This command will start a new container based on the Ubuntu 20.04 image, and open a bash shell inside the container. From here, you can use the container as you would any other Linux system, installing and running any software you need.

# Comparing to Docker

In addition to running containers, Podman also provides a number of other useful features for software development. For example, you can use it to build container images from Dockerfiles, or to push and pull container images to and from container image registries.

Here is an example of how you can use Podman to build a container image from a Dockerfile:

```bash
$ podman build -t myapp:latest .
```

This command will build a new container image based on the instructions in the Dockerfile located in the current directory, and give it the tag "myapp:latest". You can then use this image to run containers using Podman, or push it to a container image registry for deployment.

Overall, Podman is a powerful and flexible tool for managing containers and container images, and is an excellent choice for software development. With its support for running containers directly on your host system, building and managing container images, and coordinating multiple containers, it provides a range of useful features for developing, testing, and deploying software applications in containers.

# Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)