---
layout: post
title:  Hello Salesforce DX a.k.a. Salesforce Developer Experience.
---
![_config.yml]({{ site.baseurl }}/images/HelloDX/1.png)

This blog offers you all the excitement you need as a Salesforce Developer to start with DX!
If you are already excited, get some kick by trying out the below trial which introduces installation of Force.com CLI and related software and some knowledge of GitHub.
[Get Started with Salesforce DX](https://trailhead.salesforce.com/trails/sfdx_get_started/modules/sfdx_dev_model/units/sfdx_dev_model_neworganization)

So what is all the noise about SFDX [Salesforce Developer Experience]?

It is the an effort from Salesforce to shift the traditional monolithic org development paradigm to a modern modular artifact development.


![_config.yml]({{ site.baseurl }}/images/HelloDX/2.png)

So whats artifact?
An artifact is a group of related code and customizations. An artifact can be tested independently from other components in your org. An artifact can be released independently as well. The metadata components within an artifact can only live in one artifact at a time. 

![_config.yml]({{ site.baseurl }}/images/HelloDX/3.png)

With Salesforce DX, by externalizing more of the metadata and the org shape in the form of artifacts, we can shift the app’s “source of truth” from the Salesforce org to a version control system. This standard source-driven development approach has been used by developers for years, and it’s now a core part of the Salesforce developer experience.

The artifact driven model offers the below advantages:
* Improved version control system (VCS) synchronization through change-tracking of Setup features
* The ability to improve quality and time to market through continuous integration (CI) and continuous delivery (CD)
* More fine-grained visibility and clarity into the change management of your production org
* Improving team development and collaboration.
* Facilitating automated testing and continuous integration.
* Making the release cycle more efficient and agile.
* The ability to implement more agile release management processes
* Building artifacts lets you create test plans designed specifically for the project.  

Another key innovation for Salesforce DX is something we call the scratch org. The scratch org is a brand new org type built specifically for developers and automation. It’s ephemeral, built quickly from your source and metadata, and makes it easy to build your app consistently over and over again, which is great for team collaboration and test automation.

Note: Scratch orgs aren’t a replacement for sandboxes. Sandboxes are an important part of the larger development lifecycle, and work with our new source-driven development process as the destination for packages built directly from source. All sandbox types, from developer to full, offer the ability to act as user acceptance testing (UAT) and staging environments of the production org.

This following link of Salesforce Blog could get you more idea about SFDX:
Salesforce Blog for DreamForce 2016 Session Recording for SFDX


With the introduction of the modular artifact driven development the Application Lifecycle will be as follows:
￼

Now that we have a holistic picture of the ALM lets jump into the granular details:

Dev Hub?
A Developer Hub (Dev Hub) provides you and your team with the ability to create and manage scratch orgs. Scratch orgs are temporary Salesforce environments where you do the bulk of your development work in this new source-driven development paradigm.

Scratch Orgs?
Salesforce DX introduces a new type of Salesforce environment, the scratch org, a source-driven and disposable deployment of Salesforce code and metadata. Scratch orgs are fully configurable, allowing developers to emulate different editions with different features and preferences, playing a critical role in driving developer productivity and collaboration during the development process. They can also be used as part of automated testing and implementation of a full continuous integration suite.

How to create Dev Hub Org?
To create a Dev Hub Environment, [click here](https://developer.salesforce.com/promotions/orgs/dx-signup)  

For some geek satisfaction try, App Development hands on with Salesforce DX, [click here](https://trailhead.salesforce.com/trails/sfdx_get_started/modules/sfdx_app_dev/units/sfdx_app_dev_setup_dx)

How to manage Scratch Orgs?
We could do create and delete scratch org from Force.com CLI and from Dev-Hub we could manage scratch orgs as well. Here is the screen shot for Dev-Hub.
￼

Some commands for start-up:

Prerequisites:
1. SFDC CLI [installed on PC](https://developer.salesforce.com/tools/sfdxcli)  
2. Have the Dev Hub credentials ready 

Create a Dev-Hub org first we need to authorize the devhub by web login flow. You will have the credentials if you have signed-up using the link mentioned above. 

```
sfdx force:auth:web:login -d -a DevHub
```
Here -d defines that the org by which u will login is a devhub org. without -d that will be some sandbox or other org from which u will retrieve the project or so.

To open you devhub organization:

```
sfdx force:org:open -u DevHub

```

To view all orgs including the scratch orgs, sandboxes and development orgs:

```
sfdx force:org:list
sfdx force:org:list —verbose

```

Now before we start a project and start using the Scratch orgs, let me introduce to you the latest VS Code extension from Salesforce.
Salesforce Extensions for VS Code
* Functionality to interact with the Salesforce CLI
* Access to the Apex Language Server for syntax highlighting and code completion
* Support for Lightning component bundles
* Support for Visualforce pages and components
* Support for the real-time Apex Debugger
It’s also pre-integrated with Git but can work with other version control systems.

You can get more details not he VSCode extension from [here] (https://marketplace.visualstudio.com/items?itemName=salesforce.salesforcedx-vscode)

Now lets create a dummy project using the commands below:

Create a SFDX project on your local machine:

```
sfdx force:project:create -n sfdcbrewery

```

A Salesforce DX project is a local directory structure of your artifact source and Salesforce DX metadata that lets you develop and test with Salesforce DX tooling. It contains configuration files for creating scratch orgs. It can contain data to be loaded into orgs for development or testing. It should also contain tests that you rely on to validate your artifact. At a minimum, the project manages the source for one artifact. That being said, if multiple artifacts get built and released together, you can organize these artifacts into a single SFDX project. Each of your artifacts aligns to a package directory defined in the project configuration file.

Go to the project folder and create a scratch org:

```
sfdx force:org:create -s -f config/project-scratch-def.json -a TestAppScratch

```

Note: At the time of creating scratch org to test your project, you should have many things in your mind: What about provisioned features, communities, applications, permissions, data etc. Don't worry, I will try to cover those in my future posts.

To open the Scratch org:

```
sfdx force:org:open

```

We can also view authentication information:

```
sfdx force:org:display --targetusername

```

Set default user by using username:

```
sfdx force:config:set defaultusername=yourusername@gmail.com

```
           OR
           
set default user by using alias:

```
sfdx force:config:set defaultusername=devorg

```

Now that you have the project setup you can make all the code level changes on your local machine and push them to your scratch org for testing using the below command to your scratch org.

```
sfdx force:source:push

```

Same way you can make metadata changes on your scratch org and pull them on your local machine using:

```
sfdx force:source:pull

```

You can use any VCS(Git & Github) to keep track of the development and avoid any coding conflicts. 
For hands-on CI, I recommend [Trail](https://trailhead.salesforce.com/trails/sfdx_get_started/modules/sfdx_travis_ci) which uses the power of GIt, Github and Travis CI. 

You can also use Jenkins for CI.

![_config.yml]({{ site.baseurl }}/images/HelloDX/5.png)

In one line, Salesforce DX is a rockstar to Dev heroes who love command line. However, for admins this gets very complicated and dry.

I started using it as part of my trailhead projects to be future ready. Hey it's from Salesforce, there is a value to it :)

Happy Coding!

Useful References:
1. [SFDX CLI Download](https://developer.salesforce.com/tools/sfdxcli) 
1. [Salesforce DX Development Model Trail](https://trailhead.salesforce.com/trails/sfdx_get_started/modules/sfdx_dev_model)
1. [App Development with Salesforce DX](https://trailhead.salesforce.com/trails/sfdx_get_started/modules/sfdx_app_dev)
1. [Continuous Integration Using Salesforce DX](https://trailhead.salesforce.com/trails/sfdx_get_started/modules/sfdx_travis_ci)
1. [Git and GitHub Basics](https://trailhead.salesforce.com/trails/sfdx_get_started/modules/git-and-git-hub-basics)
1. [Salesforce DX Setup Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_intro.html)
1. [Wade Wegner about SFDX](http://www.wadewegner.com/)

Keywords: #SFDCBrewery #SriharideepKolagani #SalesforceDX #SalesforceDevelopment #Salesforce #SFDCBrewery #Brewery #SalesforceOnlineTraining #SalesforceTutorials #SalesforceCertification
