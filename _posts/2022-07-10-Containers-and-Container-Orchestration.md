---

layout: posts
title: Containers and Container Orchestration
icon: fa-comment-alt
tag: docker, kubernetes, orcherstration, containers
categories: containers
toc: true
---


Today we are going to talk about container and container orchestration, and solution like docker and Kubernetes.

# Why use containers?

Developing and deploying web application relays on different stages. Each of these stages depends on multiple libraries, depending on other runtimes and specific versions of each software. Configure this environment can be really hard, and reproduce this environment across a team of developers to make sure the development will not run into problems of matching dependencies versions and stack and even deploy this environment to testing, staging. The production server could be an even worse nightmare.

Quoting Solomon Hykes, creator of Docker, “You are going to test using Python 2.7, and then it is going to run on Python 3 in production, and something weird will happen. Or you will rely on the behavior of a certain version of an SSL library, and another one will be installed. You will run your tests on Debian, and production is on Red Hat, and all sorts of weird things happen”. As the reader may already meet, developing software is increasing in complexity year by year, each time increasing the abstraction.

As a community, we started using virtualization to easily set and reproduce the environment’s configuration on which our software relies. Containers came into play just as the technology was getting attention and evolving, but that does not mean that virtualization became useless; each scenario needs a unique solution. Containers easy the job to apply those environments, assert that each piece of the software is the same across servers and developments laptops, making sure that those variables are not on the table.

Of course, we will lose some performance to increase our reproducibility, stability, security, and manage resource boundaries among applications. And talking about most of the web applications, that is exactly what it needs. Running an application on a real machine will always be faster than running over virtualization software such as KVM (Kernel-base Virtual Machine) and even faster than on a container. It is not difficult to find people using a container solution inside a virtualized system based on KVM. In further chapters of this text, we will show that the performance decreasing factor is acceptable compared to virtualization benefits.

Starting from the simpler of the benefits, scalability. Supposing, you are using a monolithic application and want to increase your availability to handle more simultaneous connections. The obvious solution is increasing your server’s brute force vertically by increasing its attributes such as disk, RAM, or CPU cores. The other option is to increase it horizontally, add more servers to work in parallel to handle the increase in traffic. With this horizontal scale, you have to manage how each server talks to each other and handle the requests. Here, the container orchestrator comes into play to do this job for you to increase using the second method.

Continuing from the Hykes quote; we need to make sure three different levels of reproducibility. First, we need that all member of the software developing team is running the same run times and dependencies levels, so the same code will behave the same way on different computers, like what we see in java, with the JVM (Java Virtual Machine), which allows it to run independently of the computer since it runs the JVM. Then, we need harmony between those developing environments and the testing environment, the staging server, and production ones for the same examples given before.

Talking about resource boundaries, we can see from the diagram below how software deployment evolves between the beginning of web applications and today. We started by deploying a couple of applications to the same server, which competes with each other for the resources. If a malfunction of one API, for example, used all the resources available on our server, the entire server would be compromised. And, entire system virtualization through Virtual machine services such as VMWare would increase the process’s overhead.

We were lying; security is even more easy to justify. If your application is containerized and separated into microservices and one of them is compromised, just this one will be compromised. If you are running on a monolithic application and running directly on the server, gaining access to part of your software could result in a whole server break down, compromising user security, and, even, data loss. The containers are isolated, sandboxed from the host OS (operating system), virtualized CPU, memory storage, and network resources at an OS level.

# Containers software

We had quite a few solutions from the beginning of the virtualization, such as Linux VServer, Solaris Containers, Open VZ, Process Containers, Linux Containers, Warden Let Me Contain That For You, and last but not least, our industry-standard as of 2020, Docker. Most of the solutions mentioned and created before Docker, in 2013, were discontinued, and docker exploded in popularity and community support. Kubernetes, as known as "k8s," has over 2500 contributors, 65000 stars, and 24000 forks on GitHub, its main repository.

![Docker stack image](/assets/imgs/dockerStack.png){:class="img-fluid"}

Docker and Kubernetes became the standard; the community is enormous, the variety of examples, people discussing them, and developing using one of those technologies are wide. That is our primary goal, we need to rely on a technology that is largely supported and well tested and capable of delivering what promises is; like Javascript, it always delivers what promises are. New promising technologies are emerging as of 2020, and the industry seems to be evolving again. Docker and Kubernetes are vastly supported by the community and has already proven to be capable of handling enterprise-level jobs, which all those other emerging software needs to prove yet.

As of 2014, Docker switched its virtualization library to one of its own, called "libcontainer" and quoting Hykes, again, from its release post on Docker blog, "First, we are introducing an execution driver API which can be used to customize the execution environment surrounding each container. This allows Docker to take advantage of the numerous isolation tools available, each with their particular tradeoffs and install base: OpenVZ, systemd-nspawn, libvirt-lxc, libvirt-sandbox, qemu/kvm, BSD Jails, Solaris Zones, and even good old chroot. This is in addition to LXC, which will continue to be available as a drive of its own." The libcontainer is a pure Go Library that accesses the kernel’s container API’s directly.

# What is Kubernetes

From this point, we already understand where Docker comes to play at our development stack and what its job is, but where Kubernetes go in this scenario? And who is better to explain itself then Kubernetes? Quoting from Kubernetes documentation: "Kubernetes is a portable, extensible, open-source platform for managing containerized workloads and services, that facilitates both declarative configuration and automation. It has a large, rapidly growing ecosystem. Kubernetes services, support, and tools are widely available." Ok, from now we are one step further from understanding the big picture. Docker comes into containerizing our application, and Kubernetes is a software to manage these containers.

![Web application deployment evolution](/assets/imgs/webApplicationDeploymentEvolution.png){:class="img-fluid"}

Kubernetes emerges in a scenario where the need to handle containers failovers, downtimes, scaling, load balancing, storage orchestration, automated rollouts, bin packing, selfhealing, secret and configuration management, and much more. In our study case, Kubernetes will load, balance all traffic, and distribute it across container replicas on different physical servers, expose our DNS name, assuring a stable environment on each server. Furthermore, storage orchestration is of the same importance for us since it will handle mounting different storage providers to handle our application needs. Taking advantage of the automated rollouts is essential to guarantee no downtimes during updates or rollbacks due to malfunctions of compromised updates; if we can deploy containers at the desired rate, checking their healthy at every step„ we can ensure that a broken container will not go into production. And, of course, bin packing allows us to tell Kubernetes the desired amount of CPU and RAM power each container will have at its disposal.

# Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)