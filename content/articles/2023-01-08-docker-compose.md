---

type: "posts"
title: Docker Compose
icon: fa-comment-alt
tags: tutorial docker compose docker-compose
categories: ["tutorial"]
toc: true
date: "2023-01-08"
type: posts
---



Docker Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application's services. Then, with a single command, you create and start all the services from your configuration.

## 

Using Compose is a great way to simplify the development, testing, and deployment of your applications. It allows you to focus on writing code, rather than spending time on the intricacies of configuring and connecting multiple containers.

Here is an example of a docker-compose.yml file that defines a simple web application consisting of a web server and a database:

```YAML
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
    environment:
      FLASK_APP: app.py
      FLASK_ENV: development
    volumes:
      - .:/code
    depends_on:
      - db
  db:
    image: postgres
    volumes:
      - db-data:/var/lib/postgresql/data
volumes:
  db-data:
```

This file defines two services: web and db. The web service is built from the current directory (the build key) and is mapped to port 5000 on the host. It also defines environment variables FLASK_APP and FLASK_ENV and mounts the current directory to /code in the container. The depends_on key specifies that the db service must be started before the web service.

The db service uses the postgres image and mounts a volume for data persistence.

To start the application, we can use the following command:

```YAML
docker-compose up
```

This will build the web service if it hasn't been built yet, and start both the web and db services. The up command also supports a --build flag to force a rebuild of the web service.

In addition to up, Compose provides several other useful commands for managing your application. For example, you can use down to stop and remove the containers, logs to view the logs of a service, and exec to execute a command in a running container.

One of the most powerful features of Compose is the ability to define networks and volume connections between services. This allows you to create a fully-contained application that is isolated from the host and other applications.

For example, you can define a network for your application and connect all the services to it. Then, you can use the service name as the hostname within the network to communicate between services. This is useful for scenarios where you want to hide the implementation details of a service from the other services.

You can also define volumes to share data between services. This is useful for scenarios where you want to persist data or share it between multiple containers.

In addition to the services key, the docker-compose.yml file also supports several other keys that allow you to further customize your application. These include:

- build: Specifies the build context for a service.
- image: Specifies the image to use for a service.
- ports: Maps ports from the container to the host.
- environment: Sets environment variables for a service.
- volumes: Mount

## Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)