+++

Categories = ["lab"]
Tags = ["cf","microservices","cloudfoundry"]
date = "2017-08-29T07:49:11-04:00"
title = "Lab: Logging and Metrics"
weight = 3

+++

### Goal

To take a previously deployed microservice and demonstrate how to bind it to backing services. 

<!--more-->

Prerequisites
--

You have successfully completed the previous lab.

### Step 1
##### PCF Metrics for Health, logging & events via the CLI

Learning about how your application is performing is critical to help you diagnose and troubleshoot potential issues. Cloud Foundry gives you options for viewing the logs.

Open the metrics dashboard at https://metrics.run.example.com/
Use you same login id/password as you did to log into PCF.

<img src="/images/pcf-metrics.png" alt="Metrics" style="width: 70%;"/>

You can Monitor your Container Metrics, Network Metrics and Events for your app. Explore your logs, which shows all your app logs streamed using the Loggregator.

<img src="/images/metrics-architecture.png" alt="Metrics" style="width: 70%;"/>

### Step 2
##### Search & Highlight Log entries

Scroll down to the Logs section in PCF Metrics. Notice that you have the option to change the sort order of the logs, use the Type selection to choose which components you want to see in the logs as well as filtering the logs by keywords and highlighting words within them. You can also select a timeframe on the timeline to further limit the logs you see within the logging area. 

Take a few minutes to navigate and interact with PCF Metrics