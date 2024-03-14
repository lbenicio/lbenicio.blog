---
type: "posts"
title: Developing with Postgres
icon: fa-comment-alt
tags: tutorial database development
categories: ["tutorial"]

date: "2022-05-29"
---


PostgreSQL, also known as Postgres, is a powerful and feature-rich open-source relational database management system. It is a popular choice for many developers and organizations, thanks to its robust set of features and excellent performance.
## Developing with Postgres

If you're new to Postgres, one of the easiest ways to get started is by using Docker and Docker Compose. Docker allows you to run applications in containers, which are lightweight and portable environments that include everything the application needs to run. Docker Compose is a tool for defining and running multi-container Docker applications.

To get started with Postgres and Docker Compose, you'll need to have Docker and Docker Compose installed on your system. You can follow the instructions on the Docker website to install them.

Once you have Docker and Docker Compose installed, you can create a docker-compose.yml file to define your Postgres service. Here is an example of a docker-compose.yml file that defines a Postgres service:

```YAML
version: '3'
services:
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

This file defines a single service, db, which uses the postgres:latest image. It sets several environment variables to configure the Postgres instance, and exposes port 5432 to the host. It also defines a volume to persist data.

To start the Postgres service, you can use the following command:


```YAML
docker-compose up
```

This will start the Postgres service in a container. You can connect to the Postgres instance using a Postgres client, such as psql or a GUI tool like pgAdmin.

Once you have the Postgres service running, you can use it as the database for your application. For example, you might have another service in your docker-compose.yml file that connects to the Postgres service and uses it to store data.

Here is an example of a docker-compose.yml file that defines a web service that connects to the Postgres service:

```YAML
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
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

This file defines two services: web and db. The web service is built from the current directory (the build key) and is mapped to port 5000 on the host. It also defines an environment variable, DATABASE_URL, that specifies the connection string for the Postgres service. The depends_on key specifies that the `db`.

## Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)