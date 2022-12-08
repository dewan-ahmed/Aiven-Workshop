---
title: "Overview of development environment data infrastructure"
chapter: true
weight: 1
---

# Overview of development environment data infrastructure

In this section, you will get an overview of the development environment you'll create consisting of the following three services:

- Aiven for PostgreSQL® service for your relational data
- Aiven for Redis®* for your non-relational or NoSQL data
- Aiven for ClickHouse® for your analytical data

Besides creating the services, you'll also create a database and a database admin user for the Aiven for PostgreSQL® service, a service inegration to the Aiven for ClickHouse® service so that the database from the PostgreSQL service is copied over to ClickHouse for analytical processing. Service integration is an Aiven concept that allows you to connect two Aiven services to exchange data without you having to glue them together with your own code. Finally, you'll create an admin user for the Redis service. The whole setup will look something like this:

![Dev Environment Data Infrastructure](/images/dev-env-data-infra.png)

### Coming up - Create the development environment using Terraform
In the next section, you'll be creating the development environment using Terraform.