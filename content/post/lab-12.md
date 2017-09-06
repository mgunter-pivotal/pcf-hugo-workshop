+++

Categories = ["lab"]
Tags = ["dotnet","microservices","cloudfoundry","steeltoe"]
date = "2017-08-29T07:49:11-04:00"
title = "Lab: Steeltoe Service Discovery and Circuit Breaker"
weight = 71

+++

### Goal

Learn how to write applications that retreive their configuration from Spring Cloud Config Server 

<!--more-->

Prerequisites
--
<!--more-->

Prerequisites
--

1. Install .NET Core 2.0

    [.NET Core](https://www.microsoft.com/net/core)

2. Clone or Download the Steeltoe Samples

	[Steeltoe Samples:  https://github.com/rossr3-pivotal/Samples](https://github.com/rossr3-pivotal/Samples)

	##### Get the source code repository app

	<img src="/images/steeltoe-samples-git.png" alt="Git style="width: 40%;"/>

	Download the source code. Download as Zip file and save it in local folder

	```bash
	unzip Samples-master.zip
	```
	##### ---OR---

	Fork and Clone

	[Steeltoe Samples:  https://github.com/rossr3-pivotal/Samples](https://github.com/rossr3-pivotal/Samples)

	For Linux/Mac:
	```bash
	$ git clone https://github.com/rossr3-pivotal/Samples.git
	```

	For Windows
	```
	C:\<Some Directory to save code>\> git clone https://github.com/rossr3-pivotal/Samples.git
	```
	
3. Environment Proxy Setup (Optional depending on network configuration)

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
	
Steps
--

In this workshop we are going to follow these steps to deploy a couple of .NET Core applications that use the Service Discovery and the Circuit Breaker Dashboard to view the health of the microservices. 

***
## Deploying the Fortune Teller Service

### Step 1
##### Build the Fortune Teller Service

By this point, you should have cloned (or forked, or downloaded) the [Steeltoe Samples:  https://github.com/rossr3-pivotal/Samples](https://github.com/rossr3-pivotal/Samples).  Let's prepare the application to deploy to Cloud Foundry. 

Change into the correct folder, which is CircuitBreaker/src/AspDotNetCore/FortuneTeller/Fortune-Teller-Service. Notice that we are going down several levels to the Fortune-Teller-Service Folder:

Linux/Mac:

   ```bash
   cd CircuitBreaker/src/AspDotNetCore/FortuneTeller/Fortune-Teller-Service
   ```

Windows:

   ```
   cd Configuration\src\AspDotNetCore\FortuneTeller/Fortune-Teller-Service
   ```
  
### Step 2
##### Login into Pivotal Cloud Foundry (if necessary)

Each participant will have their own user ids and passwords.  

  ````
  cf login -a https://api.sys.cloud.rick-ross.com --skip-ssl-validation
  Email: myuserid
  Password: ••••••••

  Select a space (or press enter to skip):
  1. development
  2. test
  3. production

  Select any one and stick to that space for the rest of the workshop.

   ````

Login to the App Console at https://app.cloud.rick-ross.com

   <img src="/images/pcf-console.png" alt="PCF App Console" style="width: 70%;"/>

### Step 3
##### Create a Config Server within Pivotal Cloud Foundry

Let's create a config server that the application will use to get configuration information from:

   ```
   cf create-service p-service-registry standard <YOUR INITIALS>-DiscoveryService
   ```

Note that this step might take a bit to complete. Please wait until you see that the last operation resulted is "create succeeded". To do that:

   ```bash
   cf services
   Getting services in org pivotal / space development as admin...
   OK
    
   name                  service              plan       bound apps   last operation
   rr-DiscoveryService   p-service-registry   standard                create in progress
   ```
    
Once this is successful, you will see this:

   ```bash
   cf services
   Getting services in org pivotal / space development as admin...
   OK
    
   name                  service              plan       bound apps   last operation
   rr-DiscoveryService   p-service-registry   standard                create succeeded
   ```

You can now proced to the next step. 
    
### Step 4
#### Prepare the .NET Core application for deployment

   ```bash
   dotnet restore --configfile nuget.config
   dotnet publish -f netcoreapp2.0 -r ubuntu.14.04-x64
   ```

### Step 6
##### Modify the Manifest to Match the Name of Your Service

In a text editor, open up the manifest.yml file and change the <YOUR INITIALS> placeholder with your initials.

   ```
   ---
   applications:
   - name: fortuneService
     # Uncomment to use C2C networking with SSL connections
     # command: ./Fortune-Teller-Service --server.urls "https://0.0.0.0:$PORT"
     env:
       ASPNETCORE_ENVIRONMENT: Development 
     services:
      - <YOUR INITIALS>-DiscoveryService
   ```
   
Be sure to save the file before continuing.

### Step 6
##### Push the app

Push the Fortune Teller Service

On Linux/Mac:

  ```bash
  $ cf push -f manifest.yml -p bin/Debug/netcoreapp2.0/ubuntu.14.04-x64/publish 
  ```
  
 On Windows: 
  
  ```bash
  cf push -f manifest.yml -p bin\Debug\netcoreapp2.0\ubuntu.14.04-x64\publish
  ```

Which will result in output of

   ```bash
   // This will give an output which is similar to this
   requested state: started
   instances: 1/1
   usage: 1G x 1 instances
   urls: fortuneservice.app.cloud.rick-ross.com
   last uploaded: Tue Sep 5 23:38:58 UTC 2017
   stack: cflinuxfs2
   buildpack: ASP.NET Core (buildpack-1.0.25)
   ```

### Step 7
##### Visit the Applicaiton in the Browser

Open a browser and visit the /api/fortunes/random endpoint. For my service, this is http://fortuneservice.app.cloud.rick-ross.com/api/fortunes/random. Yours will be different.

<img src="/images/plain-fortune-service.png" alt="Git style="width: 40%;"/>

## Deploying the Fortune Teller UI Application

### Step 1
##### Build the Fortune Teller UI Application

By this point, you should have cloned (or forked, or downloaded) the [Steeltoe Samples:  https://github.com/rossr3-pivotal/Samples](https://github.com/rossr3-pivotal/Samples).  Let's prepare the application to deploy to Cloud Foundry. 

Change into the correct folder, which is BareMicroservices/src/AspDotNetCore/FortuneTeller/Fortune-Teller-UI. Notice that we are going down several levels to the Fortune-Teller-UI Folder:

Linux/Mac:

   ```bash
   cd ../Fortune-Teller-UI
   ```

Windows:

   ```
   cd ../Fortune-Teller-UI
   ```

### Step 2
##### Create a User Provided Service in Pivotal Cloud Foundry

Let's create a User Provided Service that will point to our Fortune Teller Service application that we just deployed. 

   ```
   cf create-user-provided-service <YOUR INITIALS>-FortuneService -p "uri"
   
   uri>
   ```

When prompted to enter the uri, enter in your URL of where your application lives. In my case, it is located here: http://fortuneservice.app.cloud.rick-ross.com/

If you want to double check your work, bring up Apps Manager, navigate to your org and space and look at the service you created. 

### Step 4
#### Prepare the .NET Core application for deployment

   ```bash
   dotnet restore --configfile nuget.config
   dotnet publish -f netcoreapp2.0 -r ubuntu.14.04-x64
   ```

### Step 5
##### Modify the Manifest to Match the Name of Your Service

In a text editor, open up the manifest.yml file and change the <YOUR INITIALS> placeholders with your initials. Notice this needs the service we created earlier in this lab. 

   ```
   ---
   applications:
  - name: fortuneui
    env:
      ASPNETCORE_ENVIRONMENT: Production
    services:
      - <YOUR INITIALS>-FortuneService
   ```
    
Be sure to save the file before continuing.

### Step 6
##### Push the app

Push the Fortune Teller UI

On Linux/Mac:

  ```bash
  $ cf push -f manifest.yml -p bin/Debug/netcoreapp2.0/ubuntu.14.04-x64/publish 
  ```
  
 On Windows: 
  
  ```bash
  cf push -f manifest.yml -p bin\Debug\netcoreapp2.0\ubuntu.14.04-x64\publish
  ```

Which will result in output of

   ```bash
   // This will give an output which is similar to this
   requested state: started
   instances: 1/1
   usage: 1G x 1 instances
   urls: fortuneui.app.cloud.rick-ross.com
   last uploaded: Mon Sep 4 00:21:40 UTC 2017
   stack: cflinuxfs2
   buildpack: ASP.NET Core (buildpack-1.0.25)
   ```

### Step 7
##### Visit the Fortune UI application in a Browser

Open the application URL in a browser. You will see something similar to this:

<img src="/images/plain-fortune-ui.png" alt="Fortune UI" style="width: 70%;"/>





