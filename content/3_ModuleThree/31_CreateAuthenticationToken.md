---
title: "Create Aiven Authentication Token"
chapter: true
weight: 1
---

# Create Aiven Authentication Token

[Aiven Provider for Terraform](https://registry.terraform.io/providers/aiven/aiven/latest/docs) makes API calls to the Aiven platform to create or modify resources. An authentication token needs to accompany these calls for user verification. In this section, you will create an Aiven authentication token that can be used within Terraform.

1. Once you log in to the [Aiven console](https://console.aiven.io), click on the **User Information** button on the top-right corner.
2. From the **Authentication** tab, click on **Generate token**.
3. Give your token an optional description and provide a number for the **Max age hours**. This is the number of hours after which your token will be expired. To cover the duration of this workshop, you can set the **Max age hours** to **3**. If you don't want the token to expire, leave this field empty.
4. Click on **Generate token** and make a note of the token since you'll never be able to see the value of the token again.

Alternatively, you can generate an authentication token using [Aiven CLI](https://docs.aiven.io/docs/tools/cli/user/user-access-token.html#avn-user-access-token-create) as well.

On the top-left corner, right under the Aiven console logo, you'll see the name of your Aiven project. Make a note of it as you'll need it in the next section.

### Coming up - Create a Aiven for Redis service using Terraform
In the next section, you will create an Aiven for Redis service using Terraform.