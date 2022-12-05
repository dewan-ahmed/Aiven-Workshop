---
title: "Create an Aiven service using Terraform"
chapter: true
weight: 3
---

# Create an Aiven service using Terraform

In this section, you'll create an Aiven for Redis™* service using Terraform. If you recall, you created this service in the previous module from the console. The difference is that you'll be creating the service without clicking buttons and in a programmatic way.

{{% notice warning %}}
The examples and sample code provided in this workshop are intended to be consumed as instructional content. These will help you understand how various AWS services can be architected to build a solution while demonstrating best practices along the way. These examples are not intended for use in production environments.
{{% /notice %}}

### Create an Aiven authentication token
In the next section, you'll generate an Aiven authentication token that will be needed to make any Aiven API call using Terraform.