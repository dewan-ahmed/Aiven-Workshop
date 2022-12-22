---
title: "Create an Aiven service"
chapter: true
weight: 2
---

# Create your first Aiven service

When you create an Aiven service using the Aiven CLI, Aiven Operator or Terraform Provider for Aiven, you have to provide an authentication token - a credential that allows a user to programmatically access resources instead of using their username and password. However, when you create the service from Aiven console, you are already authenticated and don't have to provide an authentication token.

Let's create an Aiven for Redis service by following these steps:

1. Click on **Create service**.
2. Select **Redis** from the list of services, choose **AWS** from the list of cloud providers, and select a cloud region that might be closer to you.
3. Under "Select Service Plan" you can choose a plan based on your compute, storage, and backup needs. For this workshop, let's choose **Startup-8** plan.
4. Give your service a name and then review the **Service Summary** shown on the right hand side.
5. If you're satisfied, hit **Create service** and this will start the process of service creation.
6. The blinking blue status below the service name will indicate that a VM is being provisioned for your service. Once the status is a solid green, your service(s) is up and running.
7. The **connection information** provides host, port, and the credentials for you to connect to your newly created service. You can also take advantage of the **Quick connect** button which will show you commands/code to connect to the Redis service using redis-cli or a choice of programming language.

![Redis Quick Connect](/images/redis-quick-connect.png)

### Coming up - Create multiple Aiven services
In the next section, you'll create multiple Aiven services from the Aiven console and connect them with service integration.


{{% notice note %}}
Apache, Apache Kafka, Kafka, Apache Flink, Flink, Apache Cassandra, and Cassandra are either registered trademarks or trademarks of the Apache Software Foundation in the United States and/or other countries. M3, M3 Aggregator, M3 Coordinator, OpenSearch, PostgreSQL, MySQL, InfluxDB, Grafana, Terraform, and Kubernetes are trademarks and property of their respective owners. *Redis is a registered trademark of Redis Ltd. Any rights therein are reserved to Redis Ltd. Any use by Aiven is for referential purposes only and does not indicate any sponsorship, endorsement or affiliation between Redis and Aiven. All product and service names used in this website are for identification purposes only and do not imply endorsement.
{{% /notice %}}