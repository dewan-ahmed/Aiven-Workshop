---
title: "Create multiple Aiven services using Terraform"
chapter: true
weight: 3
---

# Create multiple Aiven services using Terraform

In the previous module, you created multiple Aiven services from the Aiven console and connected them using service integration. In this section, you will build the same setup but this time using Terraform.

![Multiple Aiven services together](/images/multiple-aiven-services.png)

## Define the required Terraform files

Start with an empty folder, and follow these steps to define and then create the Terraform resources.

1. First we declare a dependency on the **Aiven Terraform Provider**. Within the **required_providers** block, you mention the source of the provider and specify a certain version (check which are the current versions and update accordingly).
Following [Aiven Terraform Provider documentation](https://registry.terraform.io/providers/aiven/aiven/latest/docs), **api_token** is the only parameter for the provider configuration.

Add the following to a new **provider.tf** file:

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

You can also set the environment variable **AIVEN_TOKEN** for the **api_token** property. With this, you don't need to pass the **-var-file** flag when executing Terraform commands.
 
2. To avoid including sensitive information in source control, the variables are defined in the **variables.tf** file. You can then use a ***.tfvars** file with the actual values so that Terraform receives the values during runtime, and exclude it.

The **variables.tf** file defines the API token, the project name to use, and a variable called "service_name_prefix" that will be prepended to any service being created. This helps to identify resources across environments, for example, **dev**-pg-gcp-eu or **prod**-redis-aws-us. 

```
   variable "aiven_api_token" {
     description = "Aiven console API token"
     type        = string
   }
   
   variable "project_name" {
     description = "Aiven console project name"
     type        = string
   }


  variable "service_name_prefix" {
    description = "A string to prepend to the service name"
    type        = string
  }
```   
   
The **var-values.tfvars** file holds the actual values and is passed to Terraform using the **-var-file=** flag.

**var-values.tfvars** file:

```
   aiven_api_token = "<YOUR-AIVEN-AUTHENTICATION-TOKEN-GOES-HERE>"
   project_name    = "<YOUR-AIVEN-CONSOLE-PROJECT-NAME-GOES-HERE>"
   service_name_prefix = "<YOUR-CHOICE-OF-A-SERVICE-NAME-PREFIX>"
```

Edit the file and replace the **<..>** sections with the API token you created earlier, the name of the Aiven project that resources should be created in, and a string for the service name prefix.

3.  The following Terraform script creates the required services and service integrations.

The contents of the **services.tf** file should look like this:

```
    # PostgreSQL Service

    resource "aiven_pg" "demo-pg" {
      project                 = var.project_name
      cloud_name              = "google-northamerica-northeast1"
      plan                    = "startup-8"
      service_name            = join("-", [var.service_name_prefix, "postgres"])
      termination_protection  = false
      maintenance_window_dow  = "sunday"
      maintenance_window_time = "10:00:00"
    }

    # M3DB Service

    resource "aiven_m3db" "demo-m3db" {
      project                 = var.project_name
      cloud_name              = "google-northamerica-northeast1"
      plan                    = "startup-8"
      service_name            = join("-", [var.service_name_prefix, "m3db"])
      maintenance_window_dow  = "sunday"
      maintenance_window_time = "10:00:00"

      m3db_user_config {
        m3db_version = 1.1

        namespaces {
          name = "my_ns1"
          type = "unaggregated"
        }
      }
    }

    # Grafana Service

    resource "aiven_grafana" "demo-grafana" {
      project                 = var.project_name
      cloud_name              = "google-northamerica-northeast1"
      plan                    = "startup-8"
      service_name            = join("-", [var.service_name_prefix, "grafana"])
      maintenance_window_dow  = "sunday"
      maintenance_window_time = "10:00:00"

      grafana_user_config {
        alerting_enabled = true

        public_access {
          grafana = true
        }
      }
    }

    # PostgreSQL-M3DB Metrics Service Integration

    resource "aiven_service_integration" "postgresql_to_m3db" {
      project                  = var.project_name
      integration_type         = "metrics"
      source_service_name      = aiven_pg.demo-pg.service_name
      destination_service_name = aiven_m3db.demo-m3db.service_name
    }

    # M3DB-Grafana Dashboard Service Integration

    resource "aiven_service_integration" "m3db-to-grafana" {
      project                  = var.project_name
      integration_type         = "dashboard"
      source_service_name      = aiven_grafana.demo-grafana.service_name
      destination_service_name = aiven_m3db.demo-m3db.service_name
    }
```   
    
## Apply the Terraform configuration

The **init** command performs several different initialization steps in order to prepare the current working directory for use with Terraform. In our case, this command automatically finds, downloads, and installs the necessary Aiven Terraform provider plugins.

```
   terraform init 
```

The **plan** command creates an execution plan and shows you the resources that will be created (or modified) for you. This command does not actually create any resource; this is more like a preview.

```
   terraform plan -var-file=var-values.tfvars
```

If you're satisfied with the output of **terraform plan**, go ahead and run the **terraform apply** command which actually does the task or creating (or modifying) your infrastructure resources. 

```
   terraform apply -var-file=var-values.tfvars
```

You'll need to type in **yes** to confirm. 

The output will show you if everything worked well. You can now visit the [Aiven web console](https://console.aiven.io) and observe the service creation. A flashing blue sign beside your service indicates that your service is being provisioned. A solid green sign indicates that the service is up and running.

## Clean up

Since this was a test environment, be sure to delete the resources once you're done to avoid consuming unwanted bills. 

```
    terraform destroy -var-file=var-values.tfvars
```

You'll need to type in **yes** to confirm.