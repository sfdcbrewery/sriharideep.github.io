---
layout: post
title:  Introduction to Server Side Java Script for Marketing Cloud 
--- ![_config.yml]({{ site.baseurl }}/images/Alexa/1/SSJS.jpg)

## Introduction 
The Marketing Cloud uses JavaScript code processed by Marketing Cloud servers. Instead of using the browser to render the JavaScript on the client-side computer, Marketing Cloud executes the JavaScript on the server when rendering. While you can duplicate the functionality of AMPscript using SSJS, SSJS does not work with the DOM and will not function with exterior libraries. Instead, use libraries provided by Marketing Cloud to create SSJS that works within landing pages. All functions native to JavaScript, such as arrays, math functions, the EVAL function, and try catch blocks, work with SSJS. SSJS interacts with Marketing Cloud via several libraries. Write your code to work with these libraries in order to work with the information in your Marketing Cloud account. These libraries allow SSJS to be updated while maintaining previous versions in order to avoid breaking preexisting code. You can use all commands and syntax outlined in the JavaScript specifications as part of your SSJS offerings. We use Script tags to delineate Js blocks to be processed by the app. 
```
<script runat=server language="JavaScript" executioncontexttype="Post" executioncontextname=test>
    [Insert JavaScript Here]
</script>
```
We need to tell specify that Js needs to run on the server side as shown above. There are other parameters like ExecutionContextType(default: GET option: POST), ExecutionContextName and language(Js or AMP script) that can be specified within the script tag. 

Server-side JavaScript uses personalization tokens for subscriber and system attributes, data extension fields, variables, functions, and JavaScript code expressions. Here are quick few examples:

Accessing an Attribute Name:
```
<ctrl:field name=AttributeName />
```
Accessing a Sendable Data Extension Field:
```
<ctrl:field name=SendableDataExtensionField />
```
Accessing an Attribute Name with a Default Value:
```
<ctrl:field name=AttributeName default="Default Value" />
```
Accessing a Variable:
```
<ctrl:var name=JSVar />
```
or
```
<ctrl:var name=@AmpVar />
```

The full syntax adds default and format values:
```
<ctrl:var name=JSVar default=test format=g />
```

Evaluating an Expression:
```
<ctrl:eval >"a".toUpperCase() + "B".toLowerCase()</ctrl:eval>
```
The full syntax adds language, default, and format values:
```
<ctrl:eval language=javascript default=none format=G>MyVal.toUpper()</ctrl:eval>
```
Server-side JavaScript interacts with information from Marketing Cloud through the outlined methods and libraries. To use these libraries, you must first load the library.The Core library currently stands at version 1.1.1 and supports ECMAscript 3.0. This version represents the current, original version of the server-side JavaScript library and contains no revisions.
Platform.Load("InsertLibraryNameHere","1.2");

Server-side JavaScript supports the following methods:
Add - Invokes the web service API Create method on an API object
Remove - Invokes the web service API Delete method on an API object
Update - Invokes the web service API Update method on an API object
Retrieve - Invokes the web service API Retrieve method on an API object
Core Library server-side functions allow you to use Standard JSON and Js to interact with Marketing Cloud. Now that we know how to initialise Js Core-library, lets go ahead and do it. To make it interesting, lets user Account server side Js functions to access and modify accounts within your MC account. To do that we need to intialize the object as shown below:
```
Account.Init("myAccount");
```
This initializes an account with an external key. Now lets add a method to continue the ride.
```
var getAcct = Account.Retrieve({Property:"CustomerKey",SimpleOperator:"equals",Value:"MyAccount"});
```
This code queries across all applicable accounts and retrieves an account based on the filter criteria:
```
var query = {};

query.Filter = '{Property:"CustomerKey",SimpleOperator:"equals",Value:"MyAccount"}';
query.QueryAllAccounts = true;

var getAcct = Account.Retrieve(query);
```

To Add a new user we can do something like below:
```
var newUser = {
    "Name" : "Andrea Cruz",
    "UserID" : "acruz",
    "Password" : "PASSWORD",
    "Email" : "acruz@example.com",
    "ClientID" : 123456789,
    "DefaultBusinessUnitKey": "childBUKey",
    "AssociatedBusinessUnits" : ["childBUKey", "grandchildBUKey"]
};
var status = AccountUser.Add(newUser);
```
If you are interested in data extensions we can also initialize them. Once the Data Extension is initialized we can Add, remove, retrieve or update them.  Below are few of my favourite functions that I love to play with in terms of Data Extensions. 
This sample code returns the same rows in birthdayDE, but it limits the results to two rows and sorts the results by last name.

```
var testDE = DataExtension.Init("testDE");
var data = testDE.Rows.Lookup(["Age"], [25], 2, "LastName");
```
Date and time conversion is one of the basic biggies that every developer needs to know. Here is how we do it in SSJS:

```
var myDateTime = new Date();
var convertedDateTime = DateTime.LocalDateToSystemDate(myDateTime);

```

We can even initialize and play with the delivery profiles that we use in Email Studio.

Below is my personal awesome-crisp-list of SSJS functions and actions:
Email Functions -  Add, Check content, Remove, Update, Retrieve and Validate. 
1. Events - Retrieve 
Events on Marketing Cloud are: click, ForwardedEmail, ForwardedEmailOptIn, HardBounce, NotSent, Open, OtherBounce, Sent, SoftBounce, Survey and Unsubscribe.
1. FilterDefinition -  Create, Retrieve, Update and Delete 
1. HTTP - GET, CHTML(function for smaller devices) and POST
1. Tracking - Clicks.Retrieve, Retrieve and TotalByInterval.Retrieve(type,Open, Click , Bounce)
1. Subscriber Functions - Add, Attributes.Retrieve, Lists.Retrieve, Remove, Retrieve, Statistics, Unsubscribe, Update and Upsert
1. Triggered Send Functions - Add, Pause, Publish, Retrieve, Send and Start
1. Query Definition Functions - The Query Definition server-side JavaScript functions allow you to manipulate queries via server-side JavaScript in the following ways:

```
Add - var queryDef = {
    Name : "Example Query Definition",
    CustomerKey : "myQueryDef",
    CategoryID : 123456,
    TargetUpdateType : "Overwrite",
    TargetType : "DE",
    Target : {
        Name : "Example Target DE",
        CustomerKey : "example_target_de"
    },
    QueryText : "SELECT SubKey, Email, Name FROM [Example Target DE] where FavoriteItemID=77"
};

var status = QueryDefinition.Add(queryDef);
```

Now that we got familiar with SSJS lets go ahead and write a simple code that uses a recipients profile attributes dynamically.

```
<script runat=server>
    var subFullName = Platform.Recipient.GetAttributeValue("First Name") + " " + Platform.Recipient.GetAttributeValue("Last Name");
</script>
<br />
<br />
Hello <ctrl:var name="subFullName" />,
The subFullName variable contains the information taken from the First Name and Last Name profile attributes.

This sample code demonstrates how to display the value of the profile attributes directly in the email:

Dear <ctrl:field name="First Name" />,
<br />
Your email address is <ctrl:field name="emailaddr" />.
```

If you like this article, consider reading:
[Marketing Cloud Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.noversion.mc-programmatic-content.meta/mc-programmatic-content/ssjs_contentAreaAdd.html)
