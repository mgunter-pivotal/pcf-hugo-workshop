+++

Categories = ["lab"]
Tags = ["cf","microservices","cloudfoundry"]
date = "2017-08-29T07:49:11-04:00"
title = "Lab: Build and Deploy Apps on PCF"
weight = 1

+++

### Goal


To deploy and configure a microservice and UI, leverage the platform for monitoring & management of the microservice, and do a blue green deployment with zero downtime.

<!--more-->

Prerequisites
--

1. Java SDK 1.8+ [Java from Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

    Set the JAVA_HOME variable to the installation dir, in case it is not automatically set

2. Git CLI for [Windows](https://github.com/git-for-windows/git/releases/download/v2.9.0.windows.1/Git-2.9.0-64-bit.exe)
   or Git from [github.com](https://desktop.github.com)

3. Cloud Foundry CLI for [Mac](https://github.com/cloudfoundry/cli/releases) or [Windows](http://docs.cloudfoundry.org/devguide/installcf/install-go-cli.html#windows)

4. Gradle for build (https://projects.eclipse.org/projects/tools.buildship)

5. Clone or Download the Source Code

	[PCF Workspace:  https://github.com/Pivotal-Field-Engineering/pcf-workspace-devops/](https://github.com/Pivotal-Field-Engineering/pcf-workspace-devops/)

	##### Get the source code repository app

	<img src="/images/git-clone.png" alt="Git style="width: 70%;"/>

	Download the source code. Download as Zip file and save it in local folder

	```bash
	   unzip pcf-workspace-devops-master.zip
	```
	##### ---OR---

	Fork and Clone

	[PCF Workspace:  https://github.com/Pivotal-Field-Engineering/pcf-workspace-devops/](https://github.com/Pivotal-Field-Engineering/pcf-workspace-devops/)

	For Linux/Mac:
	```bash
	$git clone https://github.com/Pivotal-Field-Engineering/pcf-workspace-devops.git
	```

	For Windows
	```
	C:\<Some Directory to save code>\> git clone https://github.com/Pivotal-Field-Engineering/pcf-workspace-devops.git
	```

5. Environment Proxy Setup (Optional depending on network configuration)

	If you are connected to a network that requires using a proxy, you'll need to set the appropriate environment variables. In the repository there is a file called *setenv.bat*.

	Open up a terminal/command prompt, change directory to the top level folder and run the setenv.bat file.

	For Mac
	```
	export HTTP_PROXY=<your http proxy>
	export HTTPS_PROXY=<your https proxy>
	```

	For Windows
	```
	set HTTP_PROXY=<your http proxy>
	set HTTPS_PROXY=<your https proxy>
	```

6. Gradle Environment Setup (Optional depending on network configuration)

	Create or edit the proxy information in gradle.properties. 

	For example:
	```
    systemProp.http.proxyHost=yourproxy.example.com
    systemProp.http.proxyPort=8080
    systemProp.https.proxyHost=yourproxy.example.com
    systemProp.https.proxyPort=8080
	```

Steps
--

In this workshop we are going to follow these steps to deploy apps on Cloud foundry.

    - Get a Spring boot app from an existing Git Repository.
    - Deploy it to Pivotal Cloud foundry.
    - Manage the lifecycle of the application.

__NOTE__

> The instructions in this document are for Mac/Linux based CLI/Shell. If you are using Windows, you may need to adjust your slashes.

***
### Step 1
##### Build the app
By this point, you should have cloned (or forked, or downloaded) the [workspace repo](https://github.com/Pivotal-Field-Engineering/pcf-workspace-devops/).  Now you will build the project and deploy it to Cloud Foundry.

For Linux/Mac:


    cd pcf-workspace-devops
    ./gradlew clean build


Windows:


    cd pcf-workspace-devops
    gradlew.bat clean build


### Step 2
##### Login into Pivotal Cloud Foundry

Each participant will have their own user ids and passwords.  

````
cf login -a https://api.example.com --skip-ssl-validation
  Email: myuserid
  Password: ••••••••

  Select a space (or press enter to skip):
  1. development
  2. test
  3. production

  Select any one and stick to that space for the rest of the workshop.

````

Login to the App Console at https://apps.example.com

<img src="/images/pcf-console.png" alt="PCF App Console" style="width: 70%;"/>


### Step 4
##### Push the app


1. Push the cities-hello, put your initials in the app name so we don't get conflicts

    ```bash
    $ cd cities-hello
    $ cf push <yourinitials>-cities-hello
    // This will give an output which is similar to this
    requested state: started
    instances: 1/1
    usage: 512M x 1 instances
    urls: cities-hello-lactiferous-unanswerableness.run.example.com
    last uploaded: Mon Jun 15 14:53:10 UTC 2015
    stack: cflinuxfs2
    ```
2. Open the app url

    When you push the apps, it will give the url route to the app.
    <img src="/images/welcome.png" alt="Welcome to PCF Workshop" style="width: 70%;"/>

3. If you haven't already it is a good time to walk through the AppsManager:

        https://apps.example.com

##### Recap: Part 1

> Cloud Foundry Haiku </br>
  Here is my source code </br>
  Run it on the cloud for me </br>
  I do not care how</br>

##### Discussion: Part 1

* How do you push an app to the cloud today?
* How does the cloud platform understand which runtime to use to run the app?

***




***
## PART 4: Deploy Version 2 of the App


In this section we are going to do a green-blue deployment using cf plugin `autopilot`. The same can be done by executing cf commands.

<br>

### Step 18
##### Process of Blue Green Deployment

  Review the CF Document for blue green deployment link:https://docs.pivotal.io/pivotalcf/devguide/deploy-apps/blue-green.html[Using Blue-Green Deployment to Reduce Downtime and Risk]

  In summary Blue-green deployment is a release technique that reduces downtime and risk by running two identical production environments called Blue and Green.


  <img src="/images/blue-green-process.png" alt="Blue Green Deployment Process" style="width: 100%;"/>


There are three different options in this lab to do blue-green deployment. You can pick any of them.

##### Option 1

First push a new version of the app with a blue route.

    $ cd cities-hello
    // Push the app version v1 with the hostname as blue
    $ cf push <YOUR INITIALS>-cities-hello-v1 --hostname <YOUR INITIALS>-cities-hello-blue -f manifest.hello
    // Map your outside route to this blue version
    $ cf map-route <YOUR INITIALS>-cities-hello-v1 example.com --hostname <YOUR INITIALS>-cities-hello
    $ cf apps // Check the apps and the routes

Next, you can push a new version of the app with a green route.

    // Push the app version v2 with the hostname as green
    $ cf push <YOUR INITIALS>-cities-hello-v2 --hostname <YOUR INITIALS>-cities-hello-green -f manifest.yml
    // Map the outside route to this green version. Now your outside route is mapped to both blue and green
    $ cf map-route <YOUR INITIALS>-cities-hello-v2 example.com --hostname <YOUR INITIALS>-cities-hello
    // Unmap the outside route to the blue version. All the traffic is now directed to v2
    $ cf unmap-route <YOUR INITIALS>-cities-hello-v1 example.com --hostname <YOUR INITIALS>-cities-hello


##### Option 2

Cloud Foundry plugin [Autopilot](https://github.com/concourse/autopilot) does blue green deployment, albeit it takes a different approach to other zero-downtime plugins. It does not perform any complex route re-mappings instead it leans on the manifest feature of the Cloud Foundry CLI. The method also has the advantage of treating a manifest as the source of truth and will converge the state of the system towards that. This makes the plugin ideal for continuous delivery environments.

Download the latest release of the autopilot plugin from the github releases page and save it in pcf-workspace-devops folder
 (https://github.com/contraband/autopilot/releases)

  On a Mac
  ````bash
  $ cd pcf-workspace-devops // This is the folder where you have saved the autopilot plugin
  $ cf install-plugin autopilot
  ````

  On Windows:

    cd pcf-workspace-devops // This is the folder where you have saved the autopilot plugin
    cf install-plugin autopilot.exe

  Once the plugin is installed ...

    $ cd cities-hello (on Windows cd cities-hello)
    // Append the build number to the app Name
    $ nano manifest.hello // Change the app name and append the build number, on Windows use wordpad manifest.hello

<img src="/images/pcf-blue-green-b100.png" alt="Blue Green Deployment Build" style="width: 50%;"/>

    $ cf zero-downtime-push <YOUR INITIALS>-cities-hello -f manifest.hello

##### Option 3 (This is as bash script and works only on Linux/OSX)

If you would like to inject build numbers in your app names here is a script you could use to do blue green deployments in the cities-hello directory which only works on a Mac

    Usage: blue-green.sh <app-name> <build-number> <domain>


  ````bash

  $ ./blue-green.sh  cities-hello 1001 <YOUR INITIALS>
  $ cf apps // You should see your app build 1001 and the Route
  ````

  Now push the new build 1002 of the app

  ````bash
  $ ./blue-green.sh  cities-hello 1002 example.com
  $ cf apps // You should see your app build 1002 and the same route mapped to the new build

  ````

##### Discussion: Part 4

In this part of the workshop did deployment using a blue green script without any downtime.
This script / methodology can be used in your CD pipeline to build and deploy Cloud Native Apps with zero downtime.

    Discussion on how do you do Continuous Deployment and Delivery with zero downtime today.


##### Recap

In this workshop we saw how to build, deploy, bind, scale, monitor apps on Cloud foundry and manage the lifecycle of the application

<img src="/images/blue-green.png" alt="DevOps on CF" style="width: 100%;"/>


### Q/A

