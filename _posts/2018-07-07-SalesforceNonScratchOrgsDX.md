---
layout: post
title:  Salesforce CLI for non-scratch orgs 
---
![_config.yml]({{ site.baseurl }}/images/DX/dx.jpg)

This blog post takes a deep dive on how to use DX for all developer, Prod and Non-prod Salesforce environments other than scratch orgs. 

Pre-requisites:
1. Install [VS Code](https://code.visualstudio.com/download)
2. Install [Salesforce Extensions for VS Code](https://marketplace.visualstudio.com/items?itemName=salesforce.salesforcedx-vscode)

You can refer this [post](https://sfdcbrewery.github.io/HelloSalesforceDX/) for a crisp introduction on  Salesforce DX.

Now if everything is set-up, let’s get started. Create a project as shown below:
Open VScode and press cmd+Shift+p, It will give some option. Select SFDX: Create Project.

Now open the command line in the vs code and create a project using the below command

```
sfdx force:project:create --projectname Sfdcbrewery

```
![_config.yml]({{ site.baseurl }}/images/DX/1.png)

Lets set an alias for the org using the following command:

```
sfdx force:auth:web:login --setdefaultdevhubusername --setalias sfdcbrewery

```
![_config.yml]({{ site.baseurl }}/images/DX/2.png)

Now lets create a Metadata file to pull the components we want to work with as shown below:

![_config.yml]({{ site.baseurl }}/images/DX/3.png)

```
 sfdx force:mdapi:retrieve -r metadata -u sfdcbrewery -k metadata/package.xml

```
* -u : Username or alias of the  Salesforce Org 
* -r : Location where zip file should be saved
* -k : List of components to be retrieved using package.xml

As we get a Zip file, lets unzip it using the below command:

```
unzip -o metadata/unpackaged.zip -d metadata

```
![_config.yml]({{ site.baseurl }}/images/DX/4.png)

In similar way, you can deploy your metadata using the below command:

```
sfdx force:mdapi:deploy -c -d ../mdAPIZip/unpackaged -u sfdcbrewery -w 10

```
* -u : Which Salesforce Org to be used by Salesforce DX
* -f : Zip file location containing metadata and package.xml
* -c : Check only flag, Package would be validates only
* -w : Wait time in minute for operation to be completed
* -d : Folder location of non-zipped files, with package.xml in root


Resources:
1. [Jitendrazaa](https://www.jitendrazaa.com/blog/salesforce/using-salesforcedx-sfdx-with-non-scratch-orgs/)
2. [Dev blog](https://developer.salesforce.com/blogs/2018/02/getting-started-salesforce-dx-part-1-5.html)

Keywords: #SFDCBrewery #SriharideepKolagani #SalesforceDX #SalesforceDevelopment #Salesforce #SFDCBrewery #Brewery #SalesforceOnlineTraining #SalesforceTutorials #SalesforceCertification
