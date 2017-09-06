+++

Categories = ["lab"]
Tags = ["dotnet","microservices","cloudfoundry","external-services"]
date = "2017-08-29T07:49:11-04:00"
title = "Lab: .NET Core Binding to External Services"
weight = 3

+++

### Goal

Demonstrate how you bind to external services for .NET Core applications

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

In this workshop we are going to follow these steps to deploy a couple of .NET Core applications that utilize a User Provided Service to talk to each other.  

***
## Deploying the Fortune Teller Service

### Step 1
##### Build the Fortune Teller Service

By this point, you should have cloned (or forked, or downloaded) the [Steeltoe Samples:  https://github.com/rossr3-pivotal/Samples](https://github.com/rossr3-pivotal/Samples).  Let's prepare the application to deploy to Cloud Foundry. 

Change into the correct folder, which is BareMicroservices/src/AspDotNetCore/Fortune-Teller-Service/. Notice that we are going down several levels to the Fortune-Teller-Service Folder:

Linux/Mac:

   ```bash
   cd BareMicroservices/src/AspDotNetCore/Fortune-Teller-Service/
   ```

Windows:

   ```
   cd BareMicroservices\src\AspDotNetCore\Fortune-Teller-Service
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
#### Prepare the .NET Core application for deployment

   ```bash
   dotnet restore --configfile nuget.config
   dotnet publish -f netcoreapp2.0 -r ubuntu.14.04-x64
   ```
