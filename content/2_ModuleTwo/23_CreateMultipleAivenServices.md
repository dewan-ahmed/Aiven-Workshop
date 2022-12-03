---
title: "Create Multiple Aiven Services"
chapter: true
weight: 3
---

# Create Multiple Aiven Services 

In the previous section, you created one Aiven service. In this section, let's create an Aiven for PosgreSQL® service, an Aiven for M3 service, an Aiven for Grafana® service, and the related service integrations. Service integration is an Aiven specific concept that works as a glue to connect two Aiven services together. 

![Multiple Aiven services together](/images/multiple-aiven-services.png)

In the above diagram, the PostgreSQL service metrics are pushed to M3DB which is then queried by a prebuilt Grafana dashboard. 

Steps to create all the services and service integrations:

1. From the [Aiven console](https://console.aiven.io/), click on **Create service** and start by creating an Aiven for PosgreSQL® service. Choose **PosgreSQL®** as the service, **AWS** as the cloud provider, **North America - aws-us-east-1** as the cloud region, **Startup-8** as the serice plan, default value for the **Additional Storage**, and a meaningful service name. Click on **Create service** and wait for the service to be up and running. 
2. Similarly, create Aiven for M3 and Aiven for Grafana services. Wait till all three services are up and running.
3. The first service integration will connect the PostgreSQL service to the M3DB service. This is so that we can store the PostgreSQL service metrics to a time-series database like M3DB. Click on the Aiven for PostgreSQL service and then click **Set up integration**.
![Set up integration](/images/set-up-integration.png)
4. Click on **Store Metrics** box. 
![Set up integration](/images/service-integration-pg-to-m3db.png) 
5. Choose **Existing service** and select the M3DB service from the list. Click **Enable**.
5. The next service integration will send service metrics from a time-series database to a pre-built Grafana dashboard. Click on the Grafana service and then **Set up integration**.
6. Click on the **Grafana Metrics Dashboard** box.
![Set up integration](/images/service-integration-m3db-to-grafana.png)
7. Choose **Existing service** and select the M3DB service from the list. Click **Enable**.
8. You've just finished creating three data services and connected them all using service integrations.
9. From the Grafana service, you can log in to the dashboard using the credentials under the **Connection information**. From the list of available dashboards, you can find the PostgreSQL service metrics that came in via the M3DB database.