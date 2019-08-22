# Team Foxtrot Project – README for *Aberdeen City in Data* #

This README contains maintenance information on how to install and run the ‘Aberdeen City in Data’ .NET Core MVC web application. This application is database-driven and designed to provide useful data visualisation of demographic data surrounding the city of Aberdeen.
Instructions include local installation, database configuration and deployment to the Microsoft Azure cloud computing service.

A live version of the application is deployed on Microsoft Azure, and can be accessed through the following link:
http://danmvc.azurewebsites.net/

### Developers
- Daniel Harraghy (51878772)
- Kris Hansen (51881686)
- Stefka Angelova (51879195)
- Santino Majur (51878418)
- Guo Wendong (51881838)

## Contents
 - [System Requirements](#requirements)
 - [Local Installation](#localinstall)
 - [MySQL Database Configuration](#dbconfig)
 - [Deployment to Microsoft Azure](#azure)
 - [Third-party Software](#3rdparty)
 - [File System and Packages](#files)
 - [Extending the Application in Future Development](#future)

## <a id="requirements">System Requirements</a>

This C# application is built on Microsoft’s .NET Core 2.2 framework which is a cross-platform version of .NET (i.e. the application can be run on Windows, Linux and MacOS).

It is was developed using the .NET Core MVC project template and requires a connection at the backend to a MySQL server containing the application’s database schema to operate properly (instructions on configuring the database are provided within). The user interface utilises HTML5 and JavaScript and so it should be ensured that these front-end technolgies are available (and enabled) on the browser rendering the application.

## <a id="localinstall">Local Installation</a>

1.	Install Microsoft’s .NET Core 2.2 SDK onto your system (available for download at https://dotnet.microsoft.com/download/dotnet-core/2.2).
2.	Trust the HTTPS development certificate (allowing a web application to run on ‘localhost’).
 - On Windows:
    1.	Open a console window and enter the following command: `dotnet dev-certs https --trust`
    2.	Accept the installation of the certificate.
 -	On Mac:
    1. Open the Terminal application and enter the following command: `dotnet dev-certs https --trust`
    2.	If message prompts for a password, enter your user password to agree to trust the certificate.
3.	Navigate to the root application folder in the command shell (i.e. cd path/to/team_foxtrot_2019) and run the application with the following command: `dotnet run`
4.	After the application has finished building, a message should indicate the application has started. Open a browser and navigate to https://localhost:5001 to view the application.
5.	Accept cookies by clicking ‘Accept’ on the right side of the light blue notification banner.
**N.B. Web pages which retrieve database data will not display until a valid database connection has been configured (see [MySQL Database Configuration](#dbconfig)).**

## <a id="dbconfig">MySQL Database Configuration</a>

This application requires a connection to a MySQL server containing our database structure and data. The following instructions address how a MySQL server can be set up locally, the process of importing our database schema, and configuring the application’s database connection string. 

1.	Download, install and run a local MySQL server.
 - On Windows,
    1.	Navigate to https://dev.mysql.com/downloads/windows/installer/5.7.html.
    2.	Download the MSI installer packages listed first (mysql-installer-web-community-5.7.27.0.msi)
    3.	Run the installer and follow instructions to execute the installation (more information available [here](https://dev.mysql.com/doc/refman/5.6/en/windows-installation.html) if needed).
      - Choosing the ‘Developer Default’ option as the ‘Setup Type’ is recommended for maintenance purposes however only the ‘MySQL Server’ package is required to configure the database and run the web application correctly.
      - Leave the port number as 3306.
      - Make a note of the MySQL Root user password you create.
    4. After the installation process is finished, your local MySQL server instance should be running. Open a console window and navigate to the MySQL bin directory (on the drive where MySQL is installed): `cd \path\to\MySQL\bin`
    5. Run the following command: `mysql -u root -p`
    6.	Enter your MySQL root user password to start the MySQL command-line interface.
    7.	Display current database schemas by entering: `SHOW databases;`
    8.	The output should list database schemas included with the MySQL setup (`information_schema`, `mysql`, `performance_schema` and `sys`).
    9.	Type `exit` and hit enter to exit the MySQL command line.
 -	On Mac,
    1.	Navigate to https://dev.mysql.com/downloads/mysql/5.7.html.
    2.	Download the DMG Archive installer package (version 5.7).
    3.	Open the downloaded file and run the installer package. 
    4.	Follow the installer’s instructions to install MySQL (more information available [here](https://dev.mysql.com/doc/refman/5.6/en/osx-installation-pkg.html) if needed).
    5.	After the installation is successful, a message will display a temporary password created for MySQL’s root user – make a note of this password.
    6.	Open System Preferences and select the MySQL preference pane.
    7.	Click the *Start MySQL Server* button (unless the server is already running after install).
    8.	Enter your user password when prompted.
    9.	Once the server instance is running, open a Terminal window and run the following command:
`/usr/local/mysql/bin/mysql -u root -p`
    10.	Enter your noted down temporary password. This should start the MySQL command line interface.
    11.	Set your own password (e.g. ‘apples’) by running the following command:
`SET password = password('apples');`
    12.	Display current database schemas by entering:
`SHOW databases;`
    13.	The output should list database schemas included with the MySQL setup (`information_schema`, `mysql`, `performance_schema` and `sys`).
    14.	Type `exit` and hit enter to exit the MySQL command line.
    
2.	Import the application’s database schema structure and data.
 -	On Windows,
    1.	Within the \MySQL\bin\ directory, run the following command using the correct system file path to the SQL dump file contained in the project root directory:
`mysql -u root -p sample6 C:\path\to\team_foxtrot_2019\sample6.sql`
    2.	Enter your MySQL root user password when prompted.
    3.	Launch the MySQL command line: `mysql -u root -p`
    4.	Display current database schemas by entering: `SHOW databases;`
    5.	The output list should include `sample6` as a database schema.
 -	On Mac,
    1.	Run the following command using the correct system file path to the SQL dump file contained in the project root directory:
`/usr/local/mysql/bin/mysql -u root -p sample6 < path/to/team_foxtrot_2019/sample6.sql`
    2.	Enter your personal SQL server password (e.g. ‘apples’) when prompted.
    3.	Launch the MySQL command line:
`/usr/local/mysql/bin/mysql -u root -p`
    4.	Enter your personal SQL server password (e.g. ‘apples’) when prompted.
    5.	Display current database schemas by entering: `SHOW databases;`
    6.	The output list should include `sample6` as a database.
    
3.	Modify the application’s database connection string to conform to your local running MySQL server instance.
    1.	For running the application locally, only two files in the application root directory need modified – `app.config` and `appsettings.Development.json`.
    2.	First, open `app.config` and find the connection string at line 5.
    3.	Change the `server` value to `localhost` and change the `password` value to your own personal MySQL server password (e.g. ‘apples’). The resulting string should look similar to as below: 
`"server=localhost;port=3306;database=sample6;user=root;password=[YOUR_PASSWORD];CharSet=utf8;Trusted_Connection=True;"`
    4.	Save the file and open the `appsettings.Development.json` file.
    5.	Find the connection string at line 3 and make the same changes as above then save the file (N.B. the same changes can also be made to the general `appsettings.json` file to configure the application for all runtime environments).
    
The local MySQL database instance should now be correctly configured with the application (i.e. all web pages rendering database data should display properly).

## <a id="azure">Deployment to Microsoft Azure</a>

Microsoft Azure provides a cloud platform where .NET Core applications can be hosted. The steps below describe how the application can be packaged and deployed to Azure for free by using Visual Studio 2019 or Visual Studio for Mac. 
*N.B. If neither of these are installed, follow [these](https://docs.microsoft.com/en-us/visualstudio/install/install-visual-studio?view=vs-2019) instructions to install Visual Studio 2019 (on Windows) or [these](https://docs.microsoft.com/en-us/visualstudio/mac/installation?view=vsmac-2019) instructions to install Visual Studio for Mac.*

1.	Create a free Azure account.
    1.	Navigate to https://azure.microsoft.com/en-us/free/dotnet/.
    2.	Click the *Start free* button.
    3.	Sign in with a valid Microsoft account.
    4.	Follow onscreen instructions to set up your account (credit card details required but no charges will be made).
2.	After set-up is complete, navigate to https://portal.azure.com.
    1.	Select *App Services* in the left-hand sidebar.
    2.	Click *Add* to set up a new app web app service.
    3.	Fill in the required details.
  - Create a New Resource group if one does not exist.
  - Enter a name for the application.
  - Publish as *Code*.
  -	Select .NET Core 2.2 as Runtime stack.
  -	Select the Region closest to your location.
  -	Under *App Service Plan*, 
     -	Select *Create new* and enter an App Service Plan name.
     - Click *Change size* then, in the *Dev / Test* tab, select the F1 pricing tier for a plan where you will not be charged.
    4.	Click *Review and create* then *Create*.
3.	Deploy the application to Azure from Visual Studio
  - On Windows,
    1. Launch Visual Studio.
    2. Open the project application file from the project root directory (`abdndata.csproj`)
    3. Right-click on the project (`abdndata`) in Solution Explorer and select *Publish*.
    4. In the *Publish* window, select *Azure App Service* and *Select Existing*.
    5. Choose your new Microsoft Azure App Service and click *Publish*.
  - On Mac, 
    1. Launch Visual Studio for Mac.
    2. Open the application from the project root directory (`abdndata.csproj`)
    3. Right-click on the project (`abdndata`) in Solution Explorer, select *Publish* then *Publish to Azure*.
    4. Your new Microsoft Azure App Service should be displayed. Select this and click *Publish*.
4.	After publishing is complete, the deployed app should automatically open in a new browser window (N.B. make sure your MySQL server is running to access and use all application functionalities). 

## <a id="3rdparty">Third-party Software</a>

As part of the user interface, this application integrates interactive maps using the Google Maps JavaScript API (available at https://cloud.google.com/maps-platform/). There is also a bar chart rendering live data which uses the Google Charts JavaScript library (available at https://developers.google.com/chart/) and jQuery (available at https://jquery.com/download/). Another JavaScript library used is jsPDF which renders PDF documents from JavaScript (available at https://parall.ax/products/jspdf). Finally, the application’s CSS styling is provided to a large extent by the Bootstrap CSS framework (available at https://getbootstrap.com/).

## <a id="files">File System and Packages</a>

The file system architecture of the application largely resembles that of Microsoft’s .NET Core MVC project template. HTML views, controller files and model classes are each sorted in separate folders. At the root directory, the `appsettings.json` file stores the database connection string which is called in the `Startup.cs` file (i.e. the class triggered at application launch which registers key services and configures routing) to open a new connection. The `wwwroot` folder stores the application’s assets. This includes CSS files, JavaScript libraries, JSON files and images. 

A project test folder for testing is also present at the root directory (`abdndata.Tests`). This contains subfolders containing unit tests and Selenium integration tests (for testing both local and deployed instances of the application) which were used in development. The `Interfaces` and `Repository` folders at the root directory are also connected to application testing. 'Interfaces' contain methods which are implemented in 'repositories' which themselves access and hold database data in memory for the purpose of unit testing.

The application references various NuGet packages which are listed below:
- *Microsoft.AspNetCore.App* – provides the default set of APIs for building a .NET Core application.
- *Microsoft.AspNetCore.Razor.Design* – provides MSBuild support for the Razor markup syntax for viewfiles. 
- *Microsoft.VisualStudio.Web.CodeGeneration.Design* - a code generation tool which allows controllers and views to be scaffolded from model classes.
- *Microsoft.VisualStudio.QualityTools.UnitTestFramework* – a Microsoft Visual Studio testing framework which was used to structure selenium test methods.
- *MySql.Data* – used for opening a connection to MySQL within the application and re-engineering models from the MySQL database schema.
- *Newtonsoft.Json* – A JSON framework for .NET which allows object data to be converted to a JSON format.
- *Humanizer* – provides an extension method which can reformat column names to a more readable form (i.e. capitalise and insert spaces) without hard-coding.
- *MSTest.TestFramework* – this is a dependency for *Microsoft.VisualStudio.QualityTools.UnitTestFramework*.
- *Selenium.WebDriver* – provides .NET bindings for the Selenium WebDriver API for performing Selenium testing.
- *Selenium.Support* – contains .NET support tools for Selenium WebDriver.
- *Selenium.WebDriver.ChromeDriver* – installs Chrome Driver which allows the Selenium WebDriver to automate the Chrome browser when running tests.
- *xunit* – a developer testing framework used for controller unit tests.
- *Moq* – a mocking framework for .NET Core (allows ‘mock’ objects to be created for the purpose of test code).

## <a id="future">Extending the Application in Future Development</a>

There are many options future developers have available to adapt or extend this application. First of all, the .NET Core framework was optimised to support scalability, straightforward deployment and the use of microservices (find out more [here](https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/index)). The use of a microservices infrastructure can be adopted if the application is scaled to a size which warrants different technologies to be divided into separate dynamically scalable microservices.

Other datasets associated with Aberdeen city datazones or council wards can easily be added to the corresponding tables in our MySQL database (this can be done simply with a MySQL client application such as [MySQL Workbench](https://dev.mysql.com/downloads/workbench/)). Similarly, new tables of data can also be added without the need to drop and recreate the database schema. The Entity Framework Core ORM can then be used to re-engineer new model classes from added tables. Following this, the *Microsoft.VisualStudio.Web.CodeGeneration.Design* package can be used to speedily scaffold controllers and views from the generated model classes and swiftly display the database data straight to the user interface.

Furthermore, there are many options to extend the graphical visualisation of our data. For example, regarding the usage of the Google Maps JavaScript API, the existing setup allows the developer to choose any numerical database table column connected to datazones or council wards and render this information as a heatmap which plots the given zones on a map of Aberdeen using GeoJSON data. There is scope to refactor this functionality using AJAX (Asynchronous JavaScript and XML) requests since the current implementation requires a page refresh to display different datasets. For an example of how this can be accomplished, future developers can look to the application’s Google Chart (created later in development) which uses jQuery to provide AJAX functionality – rendering different bar charts which display selected data without requiring a page reload. In addition, there are numerous possibilities for the use of Google Charts to be further exploited to provide multiple different forms of data visualisation using the existing data.


