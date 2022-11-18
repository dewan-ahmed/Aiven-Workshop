---
title: "Aiven Terraform Repeatable Data Infrastructure Workshop" # MODIFY THIS TO BE THE TITLE OF YOUR WORKSHOP
chapter: true
weight: 1
---

# Aiven Terraform Repeatable Data Infrastructure Workshop <!-- CHANGE THIS TO BE THE TITLE OF YOUR WORKSHOP -->
<br>
![Partner Logo](/images/Aiven_Logo.png)  <!-- ADD YOUR PARTNER LOGO HERE USING THE INSTRUCTIONS BELOW -->
<br>

## Welcome

This hands-on workshop is part of the AWS Aiven Data Infrastructure Workshop Series. In this workshop, you will learn how to use [Aiven platform](https://console.aiven.io/signup/email) to build your data infrastructure and how to use [Terraform Provider for Aiven](https://registry.terraform.io/providers/aiven/aiven/latest/docs) to automate the build and rebuild using infrastructure as code approach. 

What do we mean by data infrastructure? Data infrastructure includes all of your services to store, transmit, and monitor the data. This can include a relational database like PostgreSQL®, a key-value store like Redis®* or Apache Cassandra®, data streaming service like Apache Kafka®, or a visualization service like Grafana®. 

Most engineering teams have disparate services running for their data services which make provisioning, configuring, and monitoring incredibly difficuly. With Aiven platform, you can deploy Apache Kafka®, Apache Cassandra®, PostgreSQL®, Apache Flink® beta, ClickHouse® (beta), OpenSearch®, MySQL, Redis®*, InfluxDB®, Grafana®, and M3 - all available in more than 23 AWS regions around the world and manageable from one single Aiven platform.

However, it's difficult to scale when you create services from a web console or via custom scripts. That's why, Terraform Provider for Aiven will help you build, configure, and tear down your infrastructure declaratively. Terraform is HashiCorp’s open source infrastructure as code tool that lets you define resources and infrastructure in human-readable files and manages your infrastructure’s lifecycle.

Aiven Platform is available on AWS Marketplace. AWS Marketplace is a digital software catalog that makes it easy to find, try, buy, deploy, and manage software that runs on AWS. 
[![Aiven on AWS Marketplace](/images/aive_on_aws_marketplace.png)](https://aws.amazon.com/marketplace/seller-profile?id=37261588-4513-4d54-9ef9-195534d74a1b)


{{% notice note %}}
Apache, Apache Kafka, Kafka, Apache Flink, Flink, Apache Cassandra, and Cassandra are either registered trademarks or trademarks of the Apache Software Foundation in the United States and/or other countries. M3, M3 Aggregator, M3 Coordinator, OpenSearch, PostgreSQL, MySQL, InfluxDB, Grafana, Terraform, and Kubernetes are trademarks and property of their respective owners. *Redis is a registered trademark of Redis Ltd. Any rights therein are reserved to Redis Ltd. Any use by Aiven is for referential purposes only and does not indicate any sponsorship, endorsement or affiliation between Redis and Aiven. All product and service names used in this website are for identification purposes only and do not imply endorsement.
{{% /notice %}}