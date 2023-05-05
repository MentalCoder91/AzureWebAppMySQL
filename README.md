# AzureWebAppMySQL
Spring Boot Aplication deployed on Azure Webapp connection with Azure MySQL Database.

***********************  Deploying the WebApp to Azure App Service************************************


Paste the below in Git Bash

AZ_RESOURCE_GROUP=azure-spring-workshop
AZ_DATABASE_NAME=spring-db
AZ_LOCATION=eastus
AZ_MYSQL_USERNAME=spring
AZ_MYSQL_PASSWORD=spring
AZ_LOCAL_IP_ADDRESS=59.96.86.231

----------------------------------------------------------------------------------------------
Create the group name 

az group create --name $AZ_RESOURCE_GROUP --location $AZ_LOCATION


----------------------------------------------------------------------------------------------
Create an Azure MYSQL database Server



az mysql server create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME \
    --location $AZ_LOCATION \
    --sku-name B_Gen5_1 \
    --storage-size 5120 \
    --admin-user $AZ_MYSQL_USERNAME \
    --admin-password $AZ_MYSQL_PASSWORD \

----------------------------------------------------------------------------------------------

Run the following command to allow firewall access from Azure resources:



az mysql server firewall-rule create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name allAzureIPs \
    --server-name $AZ_DATABASE_NAME \
    --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0 

----------------------------------------------------------------------------------------------

The MySQL server that you created earlier is empty. It has no database that you can use with the Spring Boot application. Create a new database called demo:


az mysql db create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name demo \
    --server-name $AZ_DATABASE_NAME 




---------------------------------------------------------------------------------------------


Created SBA =>          azure-app-service application.




Deploy SBA to App Service Azure -> add this maven plugin to directly deploy the app to Azure App Service


----------------------------------------------------------------------------------------------

=> JAVA 8  (Go to the folder that contains the pom.xml and execute Maven commands check whether the maven is running on compatible JAVA version.)
1. mvn com.microsoft.azure:azure-webapp-maven-plugin:1.12.0:config                -> config
2mvn package com.microsoft.azure:azure-webapp-maven-plugin:1.12.0:deploy        -> deploy


=> JAVA 17
mvn com.microsoft.azure:azure-webapp-maven-plugin:2.9.0:config    -> Makes the entry for azure plugin in the pom.xml where we can change and customize the plugin


Below will get added in the pom.xml....

<plugin>
		        <groupId>com.microsoft.azure</groupId>
		        <artifactId>azure-webapp-maven-plugin</artifactId>
		        <version>2.9.0</version>
		        <configuration>
		            <schemaVersion>v2</schemaVersion>
		            <resourceGroup>azure-spring-workshop</resourceGroup>
		            <appName>azure-sba-app</appName>
		            <pricingTier>B1</pricingTier>
		            <region>eastus</region>
		            <runtime>
		                <os>Linux</os>
		                <javaVersion>Java 17</javaVersion>
		                <webContainer>Java SE</webContainer>
		            </runtime>
		            <deployment>
		                <resources>
		                    <resource>
		                        <directory>${project.basedir}/target</directory>
		                        <includes>
		                            <include>*.jar</include>
		                        </includes>
		                    </resource>
		                </resources>
		            </deployment>
		        </configuration>
</plugin>



mvn package com.microsoft.azure:azure-webapp-maven-plugin:2.9.0:deploy




