+++
Categories = ["agenda","workshop"]
Tags = ["logistics","speakers"]
date = "2017-11-08T21:44:03+04:00"
title = "Welcome to the Pivotal Cloud Foundry Workshop for Microsoft Azure"
type = "Introduction"
weight = 1
+++

##### Date and Time
Date: **April 27 2018**

Time: **8:30 AM - 4:45 PM**

Click on the title to get the Agenda, Prerequisites and Setup for the workshop.

<!--more-->


#### Agenda
* 08:30-09:00am  Arrival and Breakfast
* 09:00-09:15am  Marketplace Observations
* 09:15-10:00am  Microservices and Decomposing the Monolith
* 10:00-10:15am  Cloud Native Concepts
* 10:15-10:45am  Introduction to Pivotal Cloud Foundry
* 10:45-11:00am  Morning Break
* 11:00-12:30pm  Cloud-native Java with Spring Boot and Spring Cloud. A look at frameworks, patterns, anti-patterns, and tools for building Java apps in a cloud-native way.
* 12:30-1:00pm  Lunch
* 1:00-1:30pm   Lab: Getting Started with Springboot and Azure Dataservices
* 1:30-4:30pm   Lab: Spring Cloud Services: Using Microservices Frameworks
* 4:30-4:45pm   Wrap up


---

#### Speakers
+ Matt Gunter - Platform Architect at Pivotal
+ Prasad Bopardikar - Platform Architect at Pivotal
+ Vince Russo - Platform Architect at Pivotal


---

#### Prerequisites
1. Java SDK 1.8 [Java from Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

          **DO NOT use Java SDK 1.9**
          
          Set the JAVA_HOME variable to the installation dir, in case it is not automatically set
          
          Add %JAVA_HOME%\bin to your PATH
          
    To verify that you have installed it properly, run the following command from a command prompt or shell:
          
    ```bash
    javac -version
    ```

2. Git CLI for [Windows](https://github.com/git-for-windows/git/releases/download/v2.9.0.windows.1/Git-2.9.0-64-bit.exe)
   or Git from [github.com](https://desktop.github.com)

    If you have installed the Git command line, you can validate the installation by running the following command:
        
    ```bash
    git --version
    ```

3. Cloud Foundry CLI for [Mac](https://github.com/cloudfoundry/cli/releases) or [Windows](http://docs.cloudfoundry.org/devguide/installcf/install-go-cli.html#windows)

    To validate the installation, run the following command from a command prompt or shell:

    ```bash
    cf --version
    ```

4. Curl for [Windows](http://winampplugins.co.uk/curl/)
   Or for [Mac] (http://pdb.finkproject.org/pdb/package.php/curl)

    To validate the installation, run the following command from a command prompt or shell:
        
    ```bash
    curl --version
    ```

5. An account on your PCF Environment

##### Setup

1. Install the Prerequisites software (cf cli)

2. Check if you are able to use the cf cli to connect your PCF  Environment.

          cf login -a https://api.sys.cloud.rick-ross.com  --skip-ssl-validation

3. Check if you are able to connect to Git repo and download / clone the repo using CLI
4. Login to the App Manager Console at

        https://app.cloud.rick-ross.com



#### EBooks
Beyond the Twelve-Factor App - https://pivotal.io/beyond-the-twelve-factor-app
Cloud Foundry: The Cloud Native Platform - https://pivotal.io/cloud-foundry-the-cloud-native-platform
Migrating to Cloud Native Application Architectures - https://pivotal.io/platform/migrating-to-cloud-native-application-architectures-ebook
