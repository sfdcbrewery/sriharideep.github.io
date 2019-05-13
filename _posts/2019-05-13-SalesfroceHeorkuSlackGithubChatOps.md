---
layout: post
title:  Sack Dev Collaboration gap with Heroku and Slack
---
![_config.yml]({{ site.baseurl }}/images/Slack/main.png)

Heroku is the world’s most effective cloud development platform for building and delivering apps. Below are the four key areas of Heroku that enable teams to deliver engaging apps:

1. Developer Experience 
2. Colloboration 
3. Data Services 
4. Security

In this blog post, lets uncover how *collaboration* can add value to Heroku Flow. Heroku Flow is one stop solution that brings together Heroku Pipelines,  Review Apps, Heroku CI (https://www.heroku.com/continuous-integration) and GitHub integrations into an easy-to-use structured workflow for continuous delivery. With no further delay, let's setup our Heroku integration with Github:
1) Login to Heroku and create an application with pipeline with defaulted to staging.
![_config.yml]({{ site.baseurl }}/images/Slack/1.png)

2) Configure automatic deploys and connect Github. 
![_config.yml]({{ site.baseurl }}/images/Slack/2.png)

3) Enable review apps which will create a new app every-time a pull request is raised in Github. Also, add a PROD app with the master branch. 
![_config.yml]({{ site.baseurl }}/images/Slack/3.png)

4) Enable Heroku CI for automated tests 

5) Open pull request with corresponding review app build and tested. Pull requests trigger Heroku CI to run test.

6) Merge the PR to deploy to staging.

7) Promote to production.
![_config.yml]({{ site.baseurl }}/images/Slack/4.png)

## Integrating Heroku with Slack:

Heroku ChatOps uses the power of [Heroku Pipelines] (https://devcenter.heroku.com/articles/pipelines) to bring a collaborative deploy workflow to Slack. As a prerequisite, we need Slack permissions to add and approve apps.
First authorize Slack using the below URL:
```
https://chatops.heroku.com/auth/slack_install

```

Just like Heroku CLI we will be using Slack Slash commands to interact with the Heroku pipelines. Slash commands are processed as shown below:

![_config.yml]({{ site.baseurl }}/images/Slack/5.png)

Now go to the SlackBot channel and use the slash commands to enable the ChatOps.

Use the below command to login to Heorku and Github form Slack Bot:

```
/h login 

```
Make sure user has a deploy permission in Heroku and write permission in Github. 

In order to deploy code to app, use the below command form the Slack Bot:

```
/h deploy PIPELINE_NAME to STAGE_NAME

```
Use the below command to deploy a specific branch:

```
/h deploy PIPELINE_NAME/BRANCH_NAME to STAGE_NAME

```
In order to deploy to multiple apps use the below command:

```
/h deploy PIPELINE_NAME to STAGE_NAME/APP_NAME

```
![_config.yml]({{ site.baseurl }}/images/Slack/6.png)

Use the below command to promote the app to Prod 
```
/h promote PIPELINE_NAME from UPSTREAM_STAGE

```
We can also route the notifcations to different channels using the below command:

```
/h route PIPELINE_NAME to #CHANNEL_NAME

```
![_config.yml]({{ site.baseurl }}/images/Slack/7.png)

This way Heroku, Github and Slack integration helps to use a simple, mobile-ready CLI for continuous delivery that works everywhere. 

## References:
1. [SLACK MEDIUM](https://medium.com/slack-developer-blog/https-medium-com-slack-developer-blog-building-heroku-chatops-for-slack-f85ef2a3a94) 
1. [Salesforce DX Development Model Trail](https://trailhead.salesforce.com/trails/sfdx_get_started/modules/sfdx_dev_model)


Keywords: #SFDCBrewery #SriharideepKolagani #Heroku #ChatOps #Salesforce #Slack #CI #CD #Github #SalesforceCertification
