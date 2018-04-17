---
layout: post
title:  Event Monitoring on Salesforce   
---
![_config.yml]({{ site.baseurl }}/images/eventmonitoring/Eventmonitoring.jpg)

Event monitoring is an API-only feature. It isn’t anywhere in the Setup area. Instead, each organization’s event log files are stored in an API object called EventLogFile. Salesforce provides an API tool called Workbench that lets you access your EventLogFile objects.
vent monitoring provides tracking for lots of types of events, including:
* Logins
* Logouts
* URI (web clicks in Salesforce Classic)
* Lightning (web clicks, performance, and errors in Lightning Experience and the Salesforce mobile app)
* Visualforce page loads
* API calls
* Apex executions
* Report exports
All these events are stored in event log files. An event log file is generated when an event occurs in your organization and is available to view and download after 24 hours. The event types you can access and how long the files remain available depends on your edition.

Event monitoring can be used to:
1. Monitor data loss
2. Increase adoption
3. Optimize performance

Lets take a scenario where you start to see business going down after one of your employee leaves the company. Event monitoring comes handy here to track any abnormal activity of the ex-employee who left the company. Event monitoring requires the “API Enabled” and “View Event Log File” permissions.

Lets start our investigation by logging into our very own [Workbench](https://workbench.developerforce.com/login.php)
1. select queries | SOQL Query.
2. Under Object, choose EventLogFile. Under Fields, select count(). Notice that the editor populates with some query text.
3. Click Query.
![_config.yml]({{ site.baseurl }}/images/eventmonitoring/1.jpg)
￼
There are a total of 15 fields in the object. EventType and LogFile are the crucial ones though.
* EventType: This field displays which event types a record represents. If you expand EventType, Picklist Values, you can see the different types of events. In our case, we’re interested in records with an EventType of Report Export.
* LogFile: This field is where the actual information you’re looking for is stored. The contents of a log file depend on the EventType. For Report Export, this field stores everything from the ID of the user that exported the report to the browser and operating system that they used to do it.  You can also view the events through REST explorer: View Events in the REST Explorer
The REST Explorer gives you access to the Salesforce REST API, a web service that lets you retrieve data from your organization.
To get more information about your organization’s Report Export events in Workbench:
1. In the top menu, select utilities | REST Explorer.
2. Replace the existing text with /services/data/v41.0/query?q=SELECT+Id+,+EventType+,+LogDate+,+LogFileLength+,+LogFile+FROM+EventLogFile+ WHERE+EventType+=+'ReportExport'
3. Click Execute.
![_config.yml]({{ site.baseurl }}/images/eventmonitoring/2.jpg)
If no reports have been exported from your organization in the past 24 hours, the totalSize field has a value of zero. Remember that it takes 24 hours for events to become available. You can export a report from your organization and try again tomorrow. Alternatively, you can replace ReportExport with a different event type in your REST query (for example, Login).
If you have some report export events, your execution returns something like this:

![_config.yml]({{ site.baseurl }}/images/eventmonitoring/3.jpg)

Why are using REST if we can use a SOQL query?
we see the other major difference between SOAP and REST when it comes to querying event log files. The returned log files are the same, but they’re presented in different formats. When you retrieve your event log files using SOAP, the result is a serialized, Base64 string. If your organization plans on using tools like Informatica to work with event log files, you want to use SOAP to retrieve your data. REST, on the other hand, deserializes the log file. It’s still not pretty, but as you’ll see in the upcoming section, other tools can transform the REST results into an easy-to-read format.


You can download event log files in several ways, including:
* Direct download via the Event Log File browser application
* cURL script
* Python script

## Download Logs from Your Browser
Using the event log file browser application is the most straightforward approach to downloading your organization’s event monitoring data. Let’s check it out.
1. Log in to your Trailhead DE organization.
2. Navigate to the [event log file browser application](https://salesforce-elf.herokuapp.com/)
3. Click Production Login.
4. Enter a date range for your search.
5. Enter an event type for your search.
6. Click Apply.
You’ll see something like this:
￼
![_config.yml]({{ site.baseurl }}/images/eventmonitoring/4.jpg)

## Download Event Log Files Using cURL Using a cURL script to download your event log files requires the following:
1. Providing your Salesforce credentials
2. Logging in using OAuth and getting an access token
3. Using a REST query to specify which logs you’re looking for
If you’re scheduling a recurring download, this step is important. You can use something like this query to filter events by the current day.

```

https://${instance}.salesforce.com/services/data/v34.0/query?q=Select+Id+,+EventType+,+LogDate+From+EventLogFile+Where+LogDate+=+${day}

```
## Download Event Log Files Using Python
If you need a more programmatic way of downloading your organization’s event log files, you can use Python scripts. One advantage of using a Python script over a cURL script is that it’s easier for Windows users to work with, but it’s also suitable for Mac and Linux users. Check this blog [post](http://www.salesforcehacker.com/2015/03/elfpy-tasty-little-script-for.html)

## Visualize Event Log File Data
[Event Monitoring Analytics App](https://www.youtube.com/watch?v=CbDML6bdUuo)
[Splunk App for Salesforce](https://splunkbase.splunk.com/app/1931/)
[FairWarning](https://appexchange.salesforce.com/listingDetail?listingId=a0N3000000B5YHjEAN)
[CloudLock and CloudLock Viewer](https://appexchange.salesforce.com/listingDetail?listingId=a0N3000000B5YcwEAF)
[New Relic Insights](https://zapier.com/apps/new-relic-insights/integrations/salesforce)
