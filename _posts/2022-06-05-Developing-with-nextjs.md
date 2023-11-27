---

layout: posts
title: Developing with NextJS
icon: fa-comment-alt
tag: javascript node nextjs next developer
categories: tutorial
toc: true
---


Next.js is a popular framework for building server-rendered React applications. It is easy to get started with and offers a number of features that make it a great choice for building web applications, including automatic code splitting, optimized performance, and easy integration with other tools and libraries.
## Developing with NextJS

One way to start developing an application using Next.js is to use Docker and Docker Compose. Docker allows you to run your application in a containerized environment, which makes it easy to deploy and manage. Docker Compose is a tool for defining and running multi-container Docker applications, which makes it easy to manage multiple containers as a single application.

To get started with Next.js and Docker Compose, you'll need to have Docker and Docker Compose installed on your system. You can follow the instructions on the Docker website to install them.

Once you have Docker and Docker Compose installed, you can create a docker-compose.yml file to define your application and its dependencies. Here is an example of a docker-compose.yml file that defines a Next.js application and a Postgres database:

```YAML
version: '3'
services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      DATABASE_URL: postgres://user:password@db:5432/db
    depends_on:
      - db
  db:
    image: postgres:latest
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: db
    ports:
      - "5432:5432"
    volumes:
      - db-data:/var/lib/postgresql/data
volumes:
  db-data:
```

This file defines two services: app and db. The app service is built from the current directory (the build key) and is mapped to port 3000 on the host. It also defines an environment variable, DATABASE_URL, that specifies the connection string for the Postgres service. The depends_on key specifies that the app service depends on the db service, and should not start until the `db`.

## Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)