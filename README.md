# SonarQube-AzureAppService-AzureDatabase

This project is to facilitate hosting [SonarQube](https://www.sonarqube.org/) in an [Azure App Service](https://azure.microsoft.com/en-us/services/app-service/) directly with an [Azure SQL Database](https://azure.microsoft.com/en-us/services/sql-database/). This does not require SonarQube to be in a Linux container. You can also use the same [HttpPlatformHandlerStartup.ps1](https://github.com/ddkserv/SonarQube-AzureAppService/blob/master/HttpPlatformHandlerStartup.ps1) and [HttpPlatformHandler](https://docs.microsoft.com/en-us/iis/extensions/httpplatformhandler/httpplatformhandler-configuration-reference) extension to host SonarQube in IIS on a hosted machine. This would eliminate the need for more complicated setup of IIS as a reverse proxy.

[![Deploy to Azure](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/)

## Getting Started

Use the ***Deploy to Azure*** button above to deploy out an Azure App Service along with the additional files from this project. SonarQube may take up to 10 minutes to start the first time.

## SonarQube default user

Username: admin
Password: admin

## In-Depth Details

After the ARM template is deployed a deployment script is executed to copy the wwwroot folder from the repository folder to the App Service wwwroot folder. It also finds the most recent release of SonarQube to download and extract into the App Service wwwroot folder. On top of that the Connection String data is extracted from the webapp and setup in Sonar properties file.

The runtime execution is made possible by the [HttpPlatformHandler](https://docs.microsoft.com/en-us/iis/extensions/httpplatformhandler/httpplatformhandler-configuration-reference). This extension will start any executable and forward requests it receives onto the port defined in HTTP\_PLATFORM\_PORT environment variable. This port is randomly chosen at each invocation. On top of that the Connection String data is extracted from the webapp and setup in Sonar properties file. A web.config file is used to tell the HttpPlatformHandler which file to execute and what parameters to pass along to the executing file.

In order to make this work the [HttpPlatformHandlerStartup.ps1](https://github.com/ddkserv/SonarQube-AzureAppService-AzureDatabase/blob/master/HttpPlatformHandlerStartup.ps1) script is executed by the HttpPlatformHandler. The script searches for the sonar.properties file and writes the port defined in the HTTP\_PLATFORM\_PORT environment variable to the properties file. It also writes the java.exe location to the wrapper.conf file. Finally it executes one of the StartSonar.bat file to start SonarQube.

## Alternative Hosting Methods

Some alternative hosting methods are below with the relevant links.

**Azure VM**  
<http://donovanbrown.com/post/how-to-setup-a-sonarqube-server-in-azure>  
<https://blogs.msdn.microsoft.com/visualstudioalmrangers/2016/10/06/easily-deploy-sonarqube-server-in-azure/>

**Azure App Service with a Linux Container**  
<https://azure.microsoft.com/en-us/resources/templates/101-webapp-linux-sonarqube-mysql/>

**Docker Image**  
<https://hub.docker.com/_/sonarqube/>

**IIS as a Reverse Proxy**  
<https://blogs.msdn.microsoft.com/visualstudioalmrangers/2016/06/04/running-sonarqube-behind-an-iis-reversed-proxy/>  
<https://jessehouwing.net/sonarqube-configure-ssl-on-windows/>

## Credits

https://github.com/vanderby/SonarQube-AzureAppService