+++

Categories = ["lab"]
Tags = ["cf","microservices","cloudfoundry"]
date = "2017-08-29T07:49:11-04:00"
title = "Lab: Binding to Services"
weight = 3

+++

### Goal

To demonostrate how to provision and bind services that a microservice can use. 

<!--more-->

Prerequisites
--

You have successfully completed the previous lab.

Steps
--

In ths Lab you will deploy a new application that requires a MySQL database. You will create an instance of a database, bind it to your applicaiton via the command line, and then see how to accomplish the same thing by using the applicaiton manifest file. 

### Step 1
##### Create a Database from Marketplace

1. Review the docs on Marketplace Services:

    [Managing Services](http://docs.pivotal.io/pivotalcf/devguide/services/managing-services.html)

2. Create a mysql service, name it as `<YOUR INITIALS>-cities-db`

    You can create the service from the `cli` or launch the App Manager-> Select the Development Space [https://apps.run.example.com](https://apps.run.example.com) and login.
    Navigate to the marketplace and see the available services.

    <img src="/images/pcf-marketplace.png" alt="Marketplace Services" style="width: 70%;"/>
##### ---OR---
    You can create the service using the CLI.

    ````bash
    $ cf marketplace // check if mysql service is available
    $ cf create-service p-mysql 100mb <YOUR INITIALS>-cities-db
    ````

3. Launch the DB console via the `Manage` link in the App Manager.  Note the database is empty.

### Step 2
##### Push the App

1. Do a cf push on cities-service. Notice that the push will fail. In the next step you can learn why.

    ````bash
    $ cd ../cities-service  (on Windows cd ..\cities-service)
    $ cf push <YOUR INITIALS>-cities-service -i 1 -m 512M -p build/libs/cities-service.jar
    ````
2. Check the logs to learn more about why the application is not starting.
    You can look at the recent logs from the cli or open up the App Console and view the log files for the app.

    ````bash
    $ cf logs <YOUR INITIALS>-cities-service --recent
    ````
    <img src="/images/pcf-console-log.png" alt="Logs for the App" style="width:70%;"/>


### Step 3
##### Manually Binding the Service Instance

1. Review the docs on [Binding a Service Instance](http://docs.pivotal.io/pivotalcf/devguide/services/bind-service.html)
2. Bind the mysql instance `<YOUR INITIALS>-cities-db` to your app cities-service
    You can bind from the App Manager or from the `cli`

    ````bash
    $ cf bind-service <YOUR INITIALS>-cities-service <YOUR INITIALS>-cities-db
    ````

3. Restart your cities-service application to inject the new database.

    ````bash
    $ cf restart <YOUR INITIALS>-cities-service
    ````

    Notice that the application is now running.

4. Check the Env variables to see if the service is bound.
    You can do it from App Manager or from the `cli`

    ````bash
    $ cf env <YOUR INITIALS>-cities-service
    ````

5. Check the MySQL database through the `Manage` link in the App Manager to see that it now contains data.



__NOTE__

>    a. This app is an Spring Cloud app which uses Spring Cloud Configuration to bind to a database service provided by the cloud platform.

>    b. Difference between app `restage` and `restart`. An app `restage` will stop your application, run the application bits through the staging process to create a new droplet, and then start the new droplet.  `restart` will simply stop your application and start it with the existing droplet.  You typically `restart` when you need your application's' environment refreshed and you typically `restage` when you need/want the `buildpack` to run without updating the application source.

<br>

### Step 4
=======
##### Binding Services via the Manifest

Next, lets push the cities-service app with a manifest to help automate deployment.

1. Review the documentation: http://docs.pivotal.io/pivotalcf/devguide/deploy-apps/manifest.html
2. Edit the application manifest  `manifest.service` in your `cities-service`

    ````bash
    $ nano manifest.service
    ````

    On Windows

    ````bash
    wordpad.exe manifest.service
    ````

3. Set the name of the app, the amount of memory, the number of instances, and the path to the .jar file.
*Be sure to name your application '<YOUR INITIALS>-cities-service' e.g. rr-cities-service *
4. Add the services binding `<YOUR INITIALS>-cities-db` to your deployment manifest for cities-service .
5. Test your manifest by re-pushing your app with no parameters:

    ````bash
    $ cf push -f manifest.service
    ````

    Notice that using a manifest, you have moved the command line parameters (number of instances, memory, etc) into the manifest.
6. Verify you can access your application via a curl request or your browser:
   You will have to get the route to your app

    For a Mac
    ````bash
       // This will list your apps and the last column is the route.
       $ cf apps
          url: instructor-cities-service.run.example.com  
          // Note - Use HTTPS
       $ curl -i -k https://instructor-cities-service.run.example.com
    ````

    For Windows
    ````
       Open the URL (e.g. https://instructor-cities-service.run.example.com) in a browser window
    ````
    We must be able to access your application at https://instructor-cities-service.run.example.com for the next steps to work properly.

__NOTE__

> The default manifest file for an app is `manifest.yml` and it if is present, it is automatically picked without specifying the manifest file option.
In this exercise we have used a different naming convention.
