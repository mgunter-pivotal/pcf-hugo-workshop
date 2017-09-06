+++

Categories = ["lab"]
Tags = ["scaling","microservices","cloudfoundry"]
date = "2017-08-29T07:49:11-04:00"
title = "Lab: Scale an Application"
weight = 4

+++

### Goal

To take a previously deployed microservice and demonstrate how to scale the applicaiton in the Application Manager and through the command line.

<!--more-->

Prerequisites
--

### Step 1
##### Scale the Application in Apps Manager

Applications can be scaled via the command line or the console. When we talk about scale, there are two different types of scale: Vertical and Horizontal. Read [Scaling Apps](http://docs.cloudfoundry.org/devguide/deploy-apps/cf-scale.html) doc on more details on scaling applications.

When you vertically scale your application, you are increasing the amount of memory made available to your application. You would vertically scale your application while profiling your app, do performance tuning and to find the best memory settings before you deploy it in production.
Scaling your application horizontally means that you are adding application instances to increase your application throughput and performance under load.

Lets vertically scale the application to 1 GB of RAM.

   ````bash
   $ cf scale <YOUR INITIALS>-cities-service -m 1G
   ````


### Step 2
##### Scale the Application in the Console

Now scale your application down to 700 MB.

Next, lets scale up your application to 2 instances

   ````bash
   $ cf scale <YOUR INITIALS>-cities-service -i 2
   ````


To check the status of your applications you can check from the command line to see how many instances your app is running and their current state

   ````bash
   $ cf app <YOUR INITIALS>-cities-service
   ````


Once the second instance as started, scale the app back down to one instance.

You can also use the Autoscaler service from the marketplace and bind it to your app, so automatically scale it up or down.
<img src="/images/pcf-autoscaler.png" alt="Autoscaler" style="width: 70%;"/>


<br>
### Step 3
##### Verify the app from the Console

To verify that the application is running, use the following curl commands (or use your browser) to retrieve data from the service or use a browser to access the URL:

  ````bash
  $ curl -i -k https://<YOUR INITIALS>-cities-service.app.cloud.rick-ross.com/cities
  ````

  ````bash
  $ curl -i -k https://<YOUR INITIALS>-cities-service.app.cloud.rick-ross.com/cities/49
  ````

  ````bash
  $ curl -i -k https://<YOUR INITIALS>-cities-service.app.cloud.rick-ross.com/cities?size=5
  ````

  For Windows, use your browser and visit the corresponding URLs.

<br>

---

## Using the Autoscaler
### Step 4 PCF Metrics for Health, logging & events 

Learning about how your application is performing is critical to help you diagnose and troubleshoot potential issues. Cloud Foundry gives you options for viewing the logs.

Open the metrics dashboard by opening up Apps Manager, navigating to your org and space, and clicking on your application. In the Instances section, you'll see a View in PCF Metrics link. Click this link.

<img src="/images/cities-service-apps-manager" alt="Metrics" sytle="width: 70%; "/>

This will open up the PCF Metrics window for your application. If you are prompted to log in again, enter the same credentials that you have used before.

<img src="/images/cities-service-pcf-metrics" alt="Metrics" style="width: 70%;"/>

You can Monitor your Container Metrics, Network Metrics and Events for your app. Explore your logs, which shows all your app logs streamed using the Loggregator.


##### Discussion

In this part of the lab we app and scaled the app. How do you horizontally scale your applications today?
