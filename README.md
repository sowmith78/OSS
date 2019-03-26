# Open-Source-Statistics
The basic concept for this project is that Cerner's leadership wants to know how many Cerner associates are consuming, creating and contributing to open source projects. The site should be able to check which Cerner associates are contributing to Cerner's open source projects, hosted on [Cerner's Github](https://github.com/cerner "Cerner's Github page"), as well as other 3rd party libraries.

**This project was restarted on June 27, 2018 due to the stakeholder's request. Stable_old branch has the previous version of the project. Stable is the current project**

***

# Project Setup

Below are instructions to aid in the setting up of this project.

**Version numbers listed below are not necessarily the only ones that work, they are just ones that work together at the time of this writing.**

## Download and Install java Development Kit
- Required Minimum Version: [JDK 8u*](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html "JDK Download")
- Add Java to your Windows Path by following steps from [Java's website](https://java.com/en/download/help/path.xml "Setup Java in Windows Path")

## Download Eclipse (must be EE version)
- Suggested Version: [Latest](https://www.eclipse.org/downloads/packages/ "Eclipse Download")

## Enable Eclipse Auto Formatter
- Download the ossw_formatting_preset.xml from the [Jira page](https://jira2.cerner.com/browse/ACADEM-11186 "Open Source Statistics Epic")
- To import formatting preset:
	- Open Eclipse
	- Go to Window > Preferences > Java > Code Style > Formatter
	- Click Import and Select the downloaded file
	- Click Apply and Close
- To enable formatting on save
	- Go to Window > Preferences > Java > Editor > Save Actions
	- Check the 'Perform the selected actions on save' checkbox
	- Check the 'Format source code' checkbox
	- Select 'Format all lines'
	- Check the 'Organize imports' checkbox
	- Click Apply and OK
	
## Download Tomcat server
- Suggested Version: [Latest Stable Release](http://tomcat.apache.org "Tomcat server download")
- Download the Windows zip, not the installer
- Unzip to C:\Program Files\Apache\tomcat, or other preferred permanent location

## Download MySQL Community
- Suggested Version: [Lastest Release](http://dev.mysql.com/downloads/file/?id=458782 "MySQL Community Download")
- Install and follow process to create 'root' user with password 'admin'

## Setup Project in Eclipse
- Clone the project in Eclipse
	- Go to File > Import > Git > Projects from Git > Click Next
	- Select ‘Clone URI’ > Copy the GitHub URL of your project. (avoid including branch into the path). > Type in the Username and Password > Click Next
- Select the stable branch
	- Select 'Deselect All' > select the stable branch > Click Next
- Choose the appropriate Directory > Click Next
- Select Import as general project > Click Next and then Finish.

## Link to Database
-There is no need of persistence.xml anymore for this project. every configuration related to the database is configured in AppConfiguration.java file.
```
   	@Bean(name = "dataSource")
    	public DataSource dataSource() {
	DriverManagerDataSource driverManagerDataSource = new DriverManagerDataSource();
	driverManagerDataSource.setDriverClassName("com.mysql.jdbc.Driver");
	driverManagerDataSource.setUrl("jdbc:mysql://localhost:3306/oss");
	driverManagerDataSource.setUsername("root");
	driverManagerDataSource.setPassword("admin");
	logger.info("Bean DataSource specifies the MySQL database properties");
	return driverManagerDataSource;
    }
```
- All you have to do is, create a schema with name oss and update your user name and password with the above properties.

## Link to Tomcat
- Window > Preferences > Server > Runtime Environments > Add
- Select Apache Tomcat (which ever version you downloaded), click next
- Set the installed directory to where the zip was unzipped
- Leave the JRE on Workbench default JRE, click finish

## Configure Eclipse Java Version
- Check that the appropriate jdk version is installed
	- Window > Preferenes > Java > Installed JREs
	- Check the box next to jdk 1.8.0_*
	- If not installed, click Add > Next (standard VM) > JRE Home > select JDK install directory(not JRE folder).
- Check project Java versions and configured correctly. Right click project > Properties
	- Java Build Path
		- jdk1.8.0_* should be in the build path
		- If not in build path
			- Click 'Add Library'
			- Select 'JRE System Library' then click Next
			- Select 'Workspace default JRE (jdk 1.8.0_*)
			- Finish
		- If JRE or JavaSE is in build path select it and click remove
 	- Java Compiler
 		- Check 'Use default compliance settings'
 		- select version 1.8
	- Project Facets
		- Change Java version to 1.8
		
## Build Maven Project
- If the project is not imported as Maven Project
	- Right click on project > configure > convert to Maven Project
- Right click project > run as > Maven Build > write goals as clean install and the click on OK. check if your build is success or not.
- After the maven build, Right click on the project and click Refresh.

## Run Project
- Right click project > run as > run on server
	- If run on server is not available
		- Right click on project > properties > project facets
		- Checkmark Dynamic Web Module, apply, and OK
		- Run on server should now be available
- Select 'choose existing server' to choose the server you set up earlier
	- If 'choose existing server' is unavailable restart your machine
- Project should run
***

# Project Notes
This section contains notes about the project. Here you can find nuances about the project that you may or may not run into when setting up, running, or using this project.

## Github Login
- This application is registered with Github to allow users to login. The account it is registered under is owned by DevCenter.
	- The JIRA has the username and password for this account
- If the Github does not work as intended while the rest of the application verify that the callback-url port in [GitHubController.java](src/main/java/com/cerner/opensource/controller/GitHubController.java) matches your port. Then verify the callback-url registered with Github matches the same.
