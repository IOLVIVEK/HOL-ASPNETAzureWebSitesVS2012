<a name="Title"></a>
# Building and Publishing ASP.NET Applications with Windows Azure Web Sites and Visual Studio 2012 #

---
<a name="Overview"></a>
## Overview ##

Web site publication and deployment has never been easier in Windows Azure. Using familiar tools such as Web Deploy or Git, and virtually no changes to the development workflow, Windows Azure Web Sites is the next step in the Microsoft Azure platform for web developers. 

In this hands-on lab, you will explore the basic elements of the **Windows Azure Web Sites** service by creating a simple [ASP.NET MVC 4](http://www.asp.net/mvc/mvc4) application, which uses scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete). Then, you will deploy it using Web Deploy from Microsoft Visual Studio 2012 and Git commit.

Starting from a simple model class and without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views. After publishing and running the solution, you will have the application database generated in your SQL Database server, together with the MVC logic and views for data manipulation.

<a name="Objectives"></a>
### Objectives ###

In this Hands-on Lab, you will learn how to:

- Create a Web Site from the Windows Azure Management Portal
- Use Microsoft Visual Studio 2012 to build a new ASP.NET MVC 4 application
- Deploy the application using Web Deploy from Visual Studio
- Create a new Web Site with Git Repository enabled to publish the ASP.NET MVC 4 application using Git

<a name="Prerequisites"></a>
### Prerequisites ###

The following is required to complete this hands-on lab:

- [Microsoft Visual Studio 2012](http://msdn.microsoft.com/vstudio/products/)
- [GIT Version Control System](http://git-scm.com/download)
- A Windows Azure subscription with the Web Sites Preview enabled - [sign up for a free trial](http://aka.ms/WATK-FreeTrial)

> **Note:** This lab was designed to use Windows 8 Operating System.

<a name="Setup"/>
### Setup ###

In order to execute the exercises in this hands-on lab you need to set up your environment.

1. Open a Windows Explorer window and browse to the lab’s **Source** folder.

1. Right click the **Setup.cmd** file and click **Run as administrator**. This will launch the setup process that will check the necessary dependencies.

>**Note:** Make sure you have checked all the dependencies for this lab before running the setup. 

---
<a name="Exercises"></a>
## Exercises ##

This hands-on lab includes the following exercises:

- [Getting Started: Creating an MVC 4 Application using Entity Framework Code First](#GettingStarted)
- [Exercise 1: Publishing an MVC 4 Application using Web Deploy](#Exercise1)
- [Exercise 2: Publishing an MVC 4 Application using Git](#Exercise2)

<a name="GettingStarted"></a>
### Getting Started: Creating an MVC 4 Application using Entity Framework Code First ###

In this section, you will create a simple ASP.NET MVC 4 web application, using MVC 4 scaffolding with Entity Framework code first to create the CRUD methods.

<a name="GettingStartedTask1"></a>
#### Task 1 – Creating an ASP.NET MVC 4 Application in Visual Studio ####

1. Open **Microsoft Visual Studio 2012** and click the **New Project** link in the start page. Otherwise use  **File** | **New** | **Project**.

	![Creating a new project](images/new-website-vs2012.png?raw=true "Crating a new project")

	_Creating a new project_

1. Create a new **ASP.NET MVC 4 Web Application** using **.NET Framework 4** and name it **MVC4Sample.Web**.

	![Creating a new ASP.NET MVC 4 Web Application](images/mvc4-sample.png?raw=true "Creating a new ASP.NET MVC 4 Web Application")

	_Creating a new ASP.NET MVC 4 Web Application_

1. Select **Internet Application** and click **OK**.

	![Choosing Internet Application](images/internet-application.png?raw=true "Choosing Internet Application")

	_Choosing Internet Application_

1. In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO). Name it _Person_ and click **Add**.

1. Open the **Person** class and insert the following three attributes.

	<!-- mark:10-14 -->
	````C#
	using System;
	using System.Collections.Generic;
	using System.Linq;
	using System.Web;

	namespace MVC4Sample.Web.Models
	{
		public class Person
		{
			public int PersonID { get; set; }

			public string FirstName { get; set; }

			public string LastName { get; set; }        
		}
	}
	````

1. **Build MVC4Sample.Web** by using the **Ctrl + Shift + B** keyboard shortcut which will save the changes and build the project.

1. In the Solution Explorer, right-click the controllers folder and select **Add | Controller**. 

1. Name the controller _PersonController_ and complete the **Scaffolding options** with the following values.	
	- In the **Template** drop-down list, select the **MVC Controller with read/write actions and views, using Entity Framework** option.
	- In the **Model class** drop-down list, select the **Person** class.
	- In the **Data Context class** list, select **\<New data context...\>**. In the dialog box displayed, set the name to **PersonContext** and click **OK**.
	- In the **Views** drop-down list, make sure that **Razor** is selected.

	![Adding the Person controller with scaffolding](images/add-person-controller.png?raw=true "Adding the Person controller with scaffolding")

	_Adding the Person controller with scaffolding_
	
1. Click **Add** to create the new controller for **Person** with scaffolding. You have now generated the controller actions as well as the views. 
		
	![After creating the Person controller with scaffolding ](images/person-scaffolding.png?raw=true "After creating the Person controller with scaffolding")

	_After creating the Person controller with scaffolding_

1. Open **PersonController** class. Notice that the full CRUD action methods have been generated automatically. 

	````C#
	//
	// POST: /Person/Create
	[HttpPost]
	public ActionResult Create(Person person)
	{
	     if (ModelState.IsValid)
	     {
	          db.People.Add(person);
	          db.SaveChanges();
	          return RedirectToAction("Index");
	     }
	     return View(person);
	}
	
	//
	// GET: /Person/Edit/5
	public ActionResult Edit(int id = 0)
	{
	     Person person = db.People.Find(id);
	     if (person == null)
	     { 
	          return HttpNotFound();
	     }
	     return View(person);
	}
	````

	_Inside the Person controller_

1. Do not close Visual Studio.

---

<a name="Exercise1"></a>
### Exercise 1: Publishing an MVC 4 Application using Web Deploy ###

In this exercise you will create a new web site from the Windows Azure Management Portal and publish the application you obtained in the Getting Started, taking advantage of the Web Deploy publishing feature provided by Windows Azure.

<a name="Ex1Task1"></a>
#### Task 1 – Creating a New Web Site from the Windows Azure Portal ####

1. Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.

	![Log on to Windows Azure portal](images/login.png?raw=true "Log on to Windows Azure portal")

	_Log on to Windows Azure Management Portal_

1. Click **New** on the command bar.

	![Creating a new Web Site](images/new-website.png?raw=true "Creating a new Web Site")

	_Creating a new Web Site_

1. Click **Web Site** and then **Quick Create**. Provide an available URL for the new web site and click **Create Web Site**.

	> **Note:** A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage. The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal. It does not include steps for setting up a database.

	![Creating a new Web Site using Quick Create](images/quick-create.png?raw=true "Creating a new Web Site using Quick Create")

	_Creating a new Web Site using Quick Create_

1. Wait until the new **Web Site** is created.

1. Once the Web Site is created click the link under the **URL** column. Check that the new Web Site is working.

	![Browsing to the new web site](images/navigate-website.png?raw=true "Browsing to the new web site")

	_Browsing to the new web site_

	![Web site running](images/website-working.png?raw=true "Web site running")

	_Web site running_

1. Go back to the portal and click the name of the web site under the **Name** column to display the management pages.

	![Opening the web site management pages](images/go-to-the-dashboard.png?raw=true "Opening the web site management pages")
	
	_Opening the Web Site management pages_

1. In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.

	> **Note:** The _publish profile_ contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method. The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled. **Microsoft WebMatrix 2**, **Microsoft Visual Web Developer** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites. 

	![Downloading the web site publish profile](images/download-publish-profile.png?raw=true "Downloading the web site publish profile")
	
	_Downloading the Web Site publish profile_

1. Download the publish profile file to a known location. Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.

	![Saving the publish profile file](images/save-link.png?raw=true "Saving the publish profile")
	
	_Saving the publish profile file_

<a name="Ex1Task2"></a>
#### Task 2 – Configuring the Database Server ####

1. You will need a SQL Database server for storing the application database. You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Server** | **Server's Dashboard**. If you do not have a server created, you can create one using the **Add** button on the command bar. Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks. Do not create the database yet, as it will be created in a later stage.

	![SQL Database Server Dashboard](images/sql-database-server-dashboard.png?raw=true "SQL Database Server Dashboard")

	_SQL Database Server Dashboard_

1. In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**. To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](images/add-client-ip-address-ok-button.png?raw=true) button.

	![Adding Client IP Address](images/add-client-ip-address.png?raw=true)

	_Adding Client IP Address_

1. Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.

	![Confirm Changes](images/add-client-ip-address-confirm.png?raw=true)

	_Confirm Changes_

<a name="Ex1Task3"></a>
#### Task 3 – Publishing an ASP.NET MVC 4 Application using Web Deploy ####

1. Go back to the MVC 4 solution. In the **Solution Explorer**,  right-click the web site project and select **Publish**.

	![Publishing the Application](images/publishing-the-application.png?raw=true "Publishing the Application")

	_Publishing the web site_

1. Import the publish profile you saved in the first task.

	![Importing the publish profile](images/importing-the-publish-profile.png?raw=true "Importing the publish profile")

	_Importing publish profile_

1. Click **Validate Connection**. Once Validation is complete click **Next**.

	> **Note:** Validation is complete once you see a green checkmark appear next to the Validate Connection button.

	![Validating connection](images/validating-connection.png?raw=true "Validating connection")

	_Validating connection_

1. In the **Settings** page, under the **Databases** section, click the button next to the **PersonContext** textbox.

	![Web deploy configuration](images/web-deploy-configuration.png?raw=true "Web deploy configuration")

	_Web deploy configuration_

1. Configure the database connection as follows:
	* In the **Server name** type your SQL Database server URL using the _tcp:_ prefix.
	* In **User name** type your server administrator login name.
	* In **Password** type your server administrator login password.
	* Type a new database name, for example: _MVC4SampleDB_.

	![Configuring destination connection string](images/configuring-destination-connection-string.png?raw=true "Configuring destination connection string")

	_Configuring destination connection string_

1. Then click **OK**. When prompted to create the database click **Yes**.

	![Creating the database](images/creating-the-database.png?raw=true "Creating the database string")

	_Creating the database_

1. Copy the connection string from Person Context to use it later. Then click **Next**.

	![Connection string pointing to SQL Database](images/sql-database-connection-string.png?raw=true "Connection string pointing to SQL Database")

	_Connection string pointing to SQL Database_

1. In the **Preview** page, click **Publish**.

	![Publishing the web application](images/publishing-the-web-application.png?raw=true "Publishing the web application")

	_Publishing the web application_

1. Once the publishing process finishes, your default browser will open the published web site.

	![Application published to Windows Azure](images/application-published-to-windows-azure.png?raw=true "Application published to Windows Azure")

	_Application published to Windows Azure_

1. Go to **/Person** to verify that the Persons views are working as expected. You can try adding a new Person to verify it is successfully saved to the database.

	![Application Running](images/application-running.png?raw=true "Application Running")

	_Add Person view_

---

<a name="Exercise2"></a>
### Exercise 2: Publishing an MVC 4 Application using Git ###

In this exercise you will publish again the web application you created in exercise 1, but this time using Git.

If you did not executed exercise 1 you can still perform this exercise by deploying the site in the **Source\Assets** folder of this lab.

<a name="Ex2Task1"></a>
#### Task 1 – Creating a New Web Site from the Windows Azure Portal ####

1. Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using your Microsoft Account credentials associated with your subscription.

	![Log on to Windows Azure portal](images/login.png?raw=true "Log on to Windows Azure portal")

	_Log on to Windows Azure Management Portal_

1. Click **New** on the command bar.

	![Creating a new Web Site](images/new-website.png?raw=true "Creating a new Web Site")

	_Creating a new Web Site_

1. Click **Web Site** and then **Quick Create**. Provide an available URL for the new web site and click **Create Web Site**.

	> **Note:** A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage. The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal. It does not include steps for setting up a database.

	![Creating a new Web Site using Quick Create](images/quick-create.png?raw=true "Creating a new Web Site using Quick Create")

	_Creating a new Web Site using Quick Create_

1. Wait until the new web site is created.

1. Once the web site is created, click the link under the **URL** column. Check that the new web site is working.

	![Browsing to the new web site](images/navigate-website.png?raw=true "Browsing to the new web site")

	_Browsing to the new web site_

	![Web site running](images/website-working.png?raw=true "Web site running")

	_Web site running_

<a name="Ex3Task2"></a>  
#### Task 2 – Setting up Git Publishing ####

1. Go back to the Windows Azure Management Portal. In the **Web Sites** section, locate the web site you created in the previous task and open its dashboard. To do this, click the web site's **Name**. 

1. At the bottom of the page, click **Set up Git publishing** link.

	![Set up Git publishing](images/set-up-git-publishing.png?raw=true "Set up Git publishing")

	_Set up Git publishing_

1. A message indicating that your Git repository is being created will appear.

	![Creating Git Repository](images/creating-git-repository.png?raw=true "Creating Git Repository")

	_Creating Git repository_

1. Wait until your Git repository is ready to use before continue with the following task.

	![Git repository ready](images/git-repository-ready.png?raw=true "Git repository ready")

	_Git repository ready_

1. Copy the **Git URL** value. You will use it later in this exercise.

<a name="Ex3Task3"></a>  
#### Task 3 – Pushing the Application to Widows Azure using Git ####

1. Open the solution you have obtained in [exercise 1](#Exercise1) with Visual Studio. Alternatively, you can open the **MVC4Sample.Web** solution located in the **Source\Assets** folder of this lab.

1. Press **CTRL+SHIFT+B** to build the solution and download the NuGet package dependencies.

1. Open Web.config and update the **PersonContext** connection string using the one obtained from [Exercise 1 - Task 3](#Ex1Task3). You can also use the following connection string replacing the placeholders.

	````XML
	<connectionStrings>
	 ...
	 <add name="PersonContext" connectionString="Data Source=tcp:{SERVER_URL};Initial Catalog=MVC4SampleDB;User ID={SERVER_ADMIN_LOGIN};Password={PASSWORD}"
		providerName="System.Data.SqlClient" />
	</connectionStrings>
	````

1. Open a new **Git Bash** console and insert the following commands. Update the _[YOUR-APPLICATION-PATH]_ placeholder with the path of the MVC 4 solution you've created in [Exercise 1](#Exercise1). 
	
	<!-- mark:1-4 -->
	````CommandPrompt
	cd "[YOUR-APPLICATION-PATH]"
	git init
	git add .
	git commit -m "Initial commit"
	````

	![Git initialization and first commit](images/git-initialization-and-first-commit.png?raw=true "Git initialization and first commit")

	_Git initialization and first commit_

1. Push your web site to the remote **Git** repository by running the following command. Replace the placeholder with the URL you obtained from the Windows Azure Management Portal. You will be prompted for your deployment password.

	<!-- mark:1-2 -->
	````CommandPrompt
	git remote add azure [GIT-CLONE-URL]
	git push azure master
	````

	![Pushing to Windows Azure](images/pushing-to-windows-azure.png?raw=true "Pushing to Windows Azure")

	_Pushing to Windows Azure_

	> **Note:** When you deploy content to the FTP host or GIT repository of a Windows Azure website you must authenticate using **deployment credentials** that you create from the website’s **Quick Start** or **Dashboard** management pages.  If you don't know your deployment credentials you can easily reset them using the management portal. Open the web site **Dashboard** page and click the **Reset deployment credentials** link. Provide a new password and click Ok. Deployment credentials are valid for use with all Windows Azure websites associated with your subscription. 

1. In order to verify the web site was successfully pushed to Windows Azure, go back to the **Windows Azure Management Portal** and click **Web Sites**.

1. Locate your **Web Site** (where you deployed the application) and click its **Name** to see the **Dashboard**.

1. Click **Deployments** to see the **deployment history**. Verify that there is an **Active Deployment** with your _"Initial Commit"_.

	![Deployment](images/deployment.png?raw=true "Deployment")

	_Active deployment_

1. Finally, click **Browse** on the bottom bar to go to the web site. 

	![Browse web site](images/browse-web-site.png?raw=true "Browse web site")

	_Browse web site_

1. If the application was successfully deployed, you will see the ASP.NET MVC 4 template's default home page.

	![Application Running in Windows Azure](images/application-published-to-windows-azure.png?raw=true "Application Running in Windows Azure")

	_Application Running in Windows Azure_
	
1. Go to **/Person** to verify that the Persons views are working as expected. You can try adding a new Person to verify it is successfully saved to the database.

	![Application Running](images/application-running.png?raw=true "Application Running")

	_Add Person view_

---
<a name="Summary"></a>
## Summary ##
In this hands-on lab, you have created a new MVC web site using MVC 4 Scaffolding and published it to Windows Azure Web Sites. Web site publication and deployment has never been easier in Windows Azure. Using familiar tools such as Web Deploy or Git, and virtually no changes to the development workflow, Windows Azure Web Sites is the next step in the Microsoft Azure platform for web developers. 
