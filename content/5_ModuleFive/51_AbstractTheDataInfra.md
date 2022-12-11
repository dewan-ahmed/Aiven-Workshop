---
title: "Abstract away the data infrastructure"
chapter: true
weight: 1
---

# Abstract away the data infrastructure

How does your production environment data infrastructure vary from the development environment? For a starter, you might be using more powerful machines for your production environment with backup enabled for the data services. You probably are using a streaming platform like Apache Kafka for getting data fast to and from external systems.

Here is the example setup:

![Prod Environment Data Infrastructure](/images/prod-env-data-infra.png)

How can you reuse the development environment as a blueprint to build out your production environment? For the Aiven platform, the compute, storage, and backup capabilities of machines are determined by the **plan** when creating services.

For example, here's a brief comparison among three Aiven for PostgreSQL® plans:

| Plan name  | CPU  | Memory  | Storage  | Backup  | High-availability  |
|---|---|---|---|---|---|
| Startup-4  | 2 CPU  | 4 GB  | 80 GB  | 2 days with PITR  | None  |
| Business-16  | 2 CPU  | 15 GB  | 350 GB  | 14 days with PITR  | High-availability pair  |
| Premium-64  | 8 CPU  | 61 GB  | 1000 GB  | up to 30 days with PITR  | 3-node high-availability set |

If you abstract away the **plan** parameter from the Terraform code, you can reuse almost all of the development environment. The system architecture will look almost identical to that of the development environment.

Only resources you'll need to add are the Apache Kafka and the Apache Kafka topics. 

### Coming up - Create the production environment using Terraform
In the next section, you'll be creating the boilerplate production environment using Terraform so that your engineering teams can reuse this template.