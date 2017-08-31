+++

Categories = ["lab"]
Tags = ["dotnet","microservices","cloudfoundry"]
date = "2017-08-29T07:49:11-04:00"
title = "Lab: Deploying .NET Applications"
weight = 2

+++

### Goal

To deploy a Microsoft Framework .NET Application and a .NET Core Application to Pivotal Cloud Foundry

<!--more-->

Prerequisites
--

1. Clone or Download the Dot Net Attendee Source Code

	[Dot Net Attendees:  https://github.com/jennymclaughlin/DotNetAttendees](https://github.com/jennymclaughlin/DotNetAttendees)

	##### Get the source code repository app

	<img src="/images/dot-net-framework-git.png" alt="Git style="width: 40%;"/>

	Download the source code. Download as Zip file and save it in local folder

	```bash
	   unzip DotNetAttendees-master.zip
	```
	##### ---OR---

	Fork and Clone

	[Dot Net Attendees:  https://github.com/jennymclaughlin/DotNetAttendees](https://github.com/jennymclaughlin/DotNetAttendees)

	For Linux/Mac:
	```bash
	$git clone https://github.com/jennymclaughlin/DotNetAttendees.git
	```

	For Windows
	```
	C:\<Some Directory to save code>\> git clone https://github.com/jennymclaughlin/DotNetAttendees.git
	```
	
2. Clone or Download the Dot Net Core Demo Source Code

	[Dot Net Core Repo:  https://github.com/jennymclaughlin/dotnetcoreCFdemo](https://github.com/jennymclaughlin/dotnetcoreCFdemo)

	##### Get the source code repository app

	<img src="/images/dot-net-core-git.png" alt="Git style="width: 40%;"/>

	Download the source code. Download as Zip file and save it in local folder

	```bash
	   unzip dotnetcoreCFdemo-master.zip
	```
	##### ---OR---

	Fork and Clone

	[Dot Net Core Repo:  https://github.com/jennymclaughlin/dotnetcoreCFdemo](https://github.com/jennymclaughlin/dotnetcoreCFdemo)

	For Linux/Mac:
	```bash
	$git clone https://github.com/jennymclaughlin/dotnetcoreCFdemo.git
	```

	For Windows
	```
	C:\<Some Directory to save code>\> git clone https://github.com/jennymclaughlin/dotnetcoreCFdemo.git
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

	
Steps
--

In this workshop we are going to follow these steps to deploy .NET Framework and .NET Core applications on Cloud foundry.

***
### Step 1
##### Build the app
By this point, you should have cloned (or forked, or downloaded) the [DotNetAttendees Repo](https://github.com/jennymclaughlin/DotNetAttendees) and the [Dot Net Core Repo](https://github.com/jennymclaughlin/dotnetcoreCFdemo).  Now you will build the project and deploy it to Cloud Foundry.

