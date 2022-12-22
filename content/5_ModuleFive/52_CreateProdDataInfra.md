---
title: "Build the production data infrastructure using Terraform"
chapter: true
weight: 2
---

# Build the production data infrastructure using Terraform

I hope you have the setup from module 4 for your development environment. Let's abstract away the plan configuration and add the Apache Kafka resources to build repeatable production environment.

## The common Terraform files

There is no change to the **provider.tf** file. Let's add a variable **plan_name** to the **variables.tf** file. 

```
variable "plan_name" {
  description = "Aiven plan type"
  type        = string
}
```


Make a copy of the **dev.tfvars** and save it as **prod.tfvars** file. Add **plan_name** to this file.

```
plan_name       = "business-4"
```

## Add specific resources for the production environment

Let's add four resources to the existing blueprint from the development environment: an Apache Kafka service and three Apache Kafka topics.

```
resource "aiven_kafka" "kafka-service" {
  project                 = var.project_name
  cloud_name              = "aws-us-east-1"
  plan                    = var.plan_name
  service_name            = "kafka-aws-us"
}

resource "aiven_kafka_topic" "kafka-topic-a" {
  project      = var.project_name
  service_name = aiven_kafka.kafka-service.service_name
  topic_name   = "kafka-topic-a"
  partitions   = 3
  replication  = 2
}

resource "aiven_kafka_topic" "kafka-topic-b" {
  project      = var.project_name
  service_name = aiven_kafka.kafka-service.service_name
  topic_name   = "kafka-topic-b"
  partitions   = 3
  replication  = 2
}

resource "aiven_kafka_topic" "kafka-topic-c" {
  project      = var.project_name
  service_name = aiven_kafka.kafka-service.service_name
  topic_name   = "kafka-topic-c"
  partitions   = 3
  replication  = 2
}
```

The final Terraform **services.tf** file will look something like this:


```
resource "aiven_pg" "postgres_service" {
  project                 = var.project_name
  service_name            = "postgres-aws-us"
  cloud_name              = "aws-us-east-1"
  plan                    = var.plan_name
}

resource "aiven_pg_database" "relational_database" {
  project       = var.project_name
  service_name  = aiven_pg.postgres_service.service_name
  database_name = "relational_database"
}

resource "aiven_pg_user" "postgres_admin_user" {
  service_name = aiven_pg.postgres_service.service_name
  project      = var.project_name
  username     = var.db_username
  password     = var.db_password
}

resource "aiven_clickhouse" "clickhouse_service" {
  project                 = var.project_name
  service_name            = "clickhouse-aws-us"
  cloud_name              = "aws-us-east-1"
  plan                    = "business-8"
}

resource "aiven_service_integration" "clickhouse_postgres_source" {
  project                  = var.project_name
  integration_type         = "clickhouse_postgresql"
  source_service_name      = aiven_pg.postgres_service.service_name
  destination_service_name = aiven_clickhouse.clickhouse_service.service_name
  clickhouse_postgresql_user_config {
    databases {
      database = aiven_pg_database.relational_database.database_name
      schema   = "public"
    }
  }
}

resource "aiven_redis" "redis_service" {
  project                 = var.project_name
  cloud_name              = "aws-us-east-1"
  plan                    = var.plan_name
  service_name            = "redis-aws-us"
}

resource "aiven_redis_user" "redis_admin_user" {
  service_name = aiven_redis.redis_service.service_name
  project      = var.project_name
  username     = var.db_username
  password     = var.db_password
}

resource "aiven_kafka" "kafka-service" {
  project                 = var.project_name
  cloud_name              = "aws-us-east-1"
  plan                    = var.plan_name
  service_name            = "kafka-aws-us"
}

resource "aiven_kafka_topic" "kafka-topic-a" {
  project      = var.project_name
  service_name = aiven_kafka.kafka-service.service_name
  topic_name   = "kafka-topic-a"
  partitions   = 3
  replication  = 2
}

resource "aiven_kafka_topic" "kafka-topic-b" {
  project      = var.project_name
  service_name = aiven_kafka.kafka-service.service_name
  topic_name   = "kafka-topic-b"
  partitions   = 3
  replication  = 2
}

resource "aiven_kafka_topic" "kafka-topic-c" {
  project      = var.project_name
  service_name = aiven_kafka.kafka-service.service_name
  topic_name   = "kafka-topic-c"
  partitions   = 3
  replication  = 2
}
```

Add the following to the **outputs.tf** file:

```
output "kafka_connect_string" {
  description = "Kafka connection string"
  value       = aiven_kafka.kafka_service.service_uri
  sensitive   = true
}

```

## Apply the Terraform configuration

The only change to how you deploy the resources is as follows:

```
   terraform plan -var-file=prod.tfvars
```


```
   terraform apply -var-file=prod.tfvars
```

## Build, reuse, modify, deploy

Now that you have a powerful abstraction for your data infrastructure template, you can build your development environment, reuse all or partial environment, add certain resources for the production environment, and deploy to the region of your choice.

### Coming up - Closing section

In the next section, we will wrap up and summarize the learnings from this workshop.