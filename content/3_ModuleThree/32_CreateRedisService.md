---
title: "Create Aiven for Redis using Terraform"
chapter: true
weight: 2
---

# Create an Aiven for Redis service using Terraform

Your Terraform files declare the structure of your infrastructure as well as required dependencies and configuration. While you can stuff these together in one file, it's ideal to keep those as separate files. 

## Define the required Terraform files

Start with an empty folder, and follow these steps to define and then create an Aiven for Redis service.

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

The **variables.tf** file defines both the API token, and the project name to use:

```
   variable "aiven_api_token" {
     description = "Aiven console API token"
     type        = string
   }
   
   variable "project_name" {
     description = "Aiven console project name"
     type        = string
   }
```   
   
The **var-values.tfvars** file holds the actual values and is passed to Terraform using the **-var-file=** flag.

**var-values.tfvars** file:

```
   aiven_api_token = "<YOUR-AIVEN-AUTHENTICATION-TOKEN-GOES-HERE>"
   project_name    = "<YOUR-AIVEN-CONSOLE-PROJECT-NAME-GOES-HERE>"
```

Edit the file and replace the **<..>** sections with the API token you created earlier, and the name of the Aiven project that resources should be created in.

3.  The following Terraform script creates a single-node Redis service on the Aiven platform.

The contents of the **redis.tf** file should look like this:

```
    # A single-node Redis service
    
    resource "aiven_redis" "single-node-aiven-redis" {
      project                 = var.project_name
      cloud_name              = "aws-us-east-1"
      plan                    = "startup-4"
      service_name            = "aws-us-redis"
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

### Coming up - Create multiple Aiven services using Terraform
In the next section, you will create multiple Aiven services and connect them using service integration - all using Aiven Terraform Provider.