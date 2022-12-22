---
title: "Build development environment data infrastructure"
chapter: true
weight: 2
---

# Build development environment data infrastructure

In this section, you will build the development environment using Terraform.

## Create the common Terraform files

At first, create an empty directory and add the common files for this project.

1. Create a file called **provider.tf** which declares the required Aiven Terraform Provider and specific version number.

```
terraform {
  required_providers {
    aiven = {
      source  = "aiven/aiven"
      version = "~> 3.9.0"
    }
  }
}

provider "aiven" {
  api_token = var.aiven_api_token
}
```

2. Create a file called **variables.tf** to declare some input variables. Input variables in Terraform are like function arguments of traditional programming languages.

```
variable "aiven_api_token" {
  description = "Aiven console API token"
  type        = string
}

variable "project_name" {
  description = "Aiven console project name"
  type        = string
}

variable "db_username" {
  description = "Database administrator username"
  type        = string
  sensitive   = true
}

variable "db_password" {
  description = "Database administrator password"
  type        = string
  sensitive   = true
}
```

3. The actual values of the **variables.tf** file will come from a ***.tfvars** file. Create a file called **dev.tfvars** and add the following:

```
aiven_api_token = YOUR_API_TOKEN_GOES_HERE
project_name    = YOUR_AIVEN_PROJECT_NAME_GOES_HERE
db_username     = "admin"
db_password     = "insecurepassword"
```

Because this is a demo, you'll be using the same database username/password for PostgreSQL and Redis. Ideally, you would access your databases using some sort of IAM or secret management tools and not using the root database credentials.

4. The actual Terraform resources are defined in a **services.tf** file. Create this file within the same folder and add the following:

```
// This creates the PostgreSQL service
resource "aiven_pg" "postgres_service" {
  project                 = var.project_name
  service_name            = "postgres-aws-us"
  cloud_name              = "aws-us-east-1"
  plan                    = "startup-4"
}

// This creates the relational PostgreSQL database
resource "aiven_pg_database" "relational_database" {
  project       = var.project_name
  service_name  = aiven_pg.postgres_service.service_name
  database_name = "relational_database"
}

// This creates the PostgreSQL admin user
resource "aiven_pg_user" "postgres_admin_user" {
  service_name = aiven_pg.postgres_service.service_name
  project      = var.project_name
  username     = var.db_username
  password     = var.db_password
}

// This creates the ClickHouse service
resource "aiven_clickhouse" "clickhouse_service" {
  project                 = var.project_name
  service_name            = "clickhouse-aws-us"
  cloud_name              = "aws-us-east-1"
  plan                    = "startup-8" 
}


// ClickHouse service integration for the PostgreSQL service as a source
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

// This creates the Redis service
resource "aiven_redis" "redis_service" {
  project                 = var.project_name
  cloud_name              = "aws-us-east-1"
  plan                    = "startup-4"
  service_name            = "redis-aws-us"
}

// This creates the Redis admin user
resource "aiven_redis_user" "redis_admin_user" {
  service_name = aiven_redis.redis_service.service_name
  project      = var.project_name
  username     = var.db_username
  password     = var.db_password
}
```

5. If input variables are like function argument, output values in Terraform are like function return values. Create a file called **outputs.tf** and add the following:

```
output "postgres_connect_string" {
  description = "Postgres database connection string"
  value       = aiven_pg.postgres_service.service_uri
  sensitive   = true
}

output "redis_connect_string" {
  description = "Redis database connection string"
  value       = aiven_redis.redis_service.service_uri
  sensitive   = true
}
```

Note that since these output values contain database credentials, these values are marked as **sensitive**. Terraform will hide values marked as sensitive in the console output. You can find the values under the Terraform state file. 

    
## Apply the Terraform configuration

The **init** command performs several different initialization steps in order to prepare the current working directory for use with Terraform. In our case, this command automatically finds, downloads, and installs the necessary Aiven Terraform provider plugins.

```
   terraform init 
```

The **plan** command creates an execution plan and shows you the resources that will be created (or modified) for you. This command does not actually create any resource; this is more like a preview.

```
   terraform plan -var-file=variables.tfvars
```

If you're satisfied with the output of **terraform plan**, go ahead and run the **terraform apply** command which actually does the task or creating (or modifying) your infrastructure resources. 

```
   terraform apply -var-file=variables.tfvars
```

You'll need to type in **yes** to confirm. 

The output will show you if everything worked well. You can now visit the [Aiven web console](https://console.aiven.io) and observe the services being created. A flashing blue sign on the Aiven console beside your service will indicate that your service is being provisioned. A solid green sign will indicate that the service is up and running. Alternatively, you can find the service URIs for the PostgreSQL and Redis services from the Terraform state file (as a result of the **outputs.tf** file) and use those programmatically to connect to these services. 

## Clean up

Since this was a test environment, be sure to delete the resources once you're done to avoid consuming unwanted bills. 

```
    terraform destroy -var-file=variables.tfvars
```

You'll need to type in **yes** to confirm.