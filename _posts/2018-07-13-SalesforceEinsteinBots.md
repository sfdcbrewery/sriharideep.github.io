---
layout: post
title:  Build and deploy your first AI powered chat bot using Einstein on Salesforce Service Cloud
---
![_config.yml]({{ site.baseurl }}/images/EBots/einstein.jpg)

## What is a Chatbot?
A chatbot is an application that simulates human conversation, either aloud or via text message. Instead of having a conversation with a person, like a sales rep or support agent, a customer can have a conversation with a computer. Whether through typing or talking, a chatbot can connect with a customer. It can influence a customer relationship.

## Key benefits of chatbots
Einstein is an NLU technology that trains chatbots to create a learning model. The learning model helps chatbots created with Salesforce understand customer interactions in a chat window. It’s the learning that leads to one of the major benefits of chatbots: automation. Automated responses and answers from NLU save people time and money. Bots can answer the same straightforward questions over and over again, leaving customer service reps more time to help customers with more complex questions or problems.  Few of the many benefits out of chatbots:
1. Quick case deflection
2. Reduced wait times
3. Saved time for agents
4. Efficient redirects for customer inquiries
5. Intelligent responses through NLU

## It’s Showtime: Hands-on demo 
Note: You’ll need to setup Live Agent, Omni-Chanel and the Web Snap-ins prior to building a bot. 
It is now time for a hands-on demo. Einstein Bots is a part of the Service Setup as it is a Service Cloud focussed feature. In Quick Search Box, type and select Einstein Bots. 
![_config.yml]({{ site.baseurl }}/images/EBots/E1.png)

Enable Einstein Bots and also go ahead change the Live agent status to active as shown in the image below.

![_config.yml]({{ site.baseurl }}/images/EBots/E2.png)

Now go ahead and start by creating a new Bot. Give a name and description to your Bot as shown below:
![_config.yml]({{ site.baseurl }}/images/EBots/E3.png)

Enter a default greeting message that is applicable to your bots use case:
![_config.yml]({{ site.baseurl }}/images/EBots/E4.png)

Enter Items which you think your customers would be interested in or need help with depending on your use case. 
![_config.yml]({{ site.baseurl }}/images/EBots/E5.png)

Once your Bot is ready, proceed and click Finish. 
![_config.yml]({{ site.baseurl }}/images/EBots/E6.png)

As you must have already seen in the home page, we have following menu items:
![_config.yml]({{ site.baseurl }}/images/EBots/E7.png)

1. Overview(Default) - The Overview page displays basic information about your bot. We can also access the Einstein Intent Management area that lets you train your bot to understand intents and keeps track of your bot’s training status. The Settings area lets you add a pause before the bot responds to your customers to simulate a human interaction. You can use a preset, optimized delay or create your own. When you’re ready to put your bot to work, you can always click Activate.
2. Dialogs - Dialogs are conversation snippets that control what your bot can do. You can associate every dialog with a dialog intent which can be trained to understand variations in customer input. During a conversation with a customer, your bot moves between several different dialogs. Each dialog handles a portion of the conversation. For example, Welcome, Main Menu, Order Status, Location and Hours, and Transfer to Agent are individual dialogs that a customer might experience as part of a single conversation with your bot.
3. Entities - Entities are a type of data that you want to collect from a customer. We provide the following system entities: Text, DateTime, Date, Money, Number, Person, Location, Organization, Percent, Boolean, and Object (standard Salesforce or custom). You can create your own custom entities as needed.
4. Slots - A slot is a container that stores a specific piece of data collected from the customer. Each slot must be associated with an entity. Since slots are containers of information, they can be used within dialog actions as both inputs and outputs and can be inserted as part of the text in messages.
5. Performance - Monitor bot performance using an Analytics app on the Performance page.

Now Lets go ahead and select one of our dialog. In this case, I am selecting EDGE link to start with. As you can see, a dialog can be message, action, rule or a question as shown in the screenshot below. 
![_config.yml]({{ site.baseurl }}/images/EBots/E8.png)

I will go ahead and create a question. Before creating a Dialog question, let us create a slot to store the customer response to our question in terms on EDGE Link as show below.

![_config.yml]({{ site.baseurl }}/images/EBots/E9.png)

Now lets proceed with the Dialog creation. Let us create a question in Link dialog that saves the response to responseLink(Text) slot and has custom static options as below:
![_config.yml]({{ site.baseurl }}/images/EBots/E10.png)

In this demo lets go with dynamic options which means a customer response will be dynamically interpreted using the best matching intent. For this to work we need to create Intent. Enter all the ways your customers phrase this intent. Einstein Bots require a minimum of 20 customer inputs but 150 or more is ideal. The more variations that you provide, the better the bot understands your customers. Einstein Intent Sets support customer inputs in English. Use correct spellings — the bot recognizes misspellings. I have added my customer inputs as shown in the picture below:
![_config.yml]({{ site.baseurl }}/images/EBots/E11.png)

 When we test the bot, it won't be able to interpret the question as you can see below:
 
 ![_config.yml]({{ site.baseurl }}/images/EBots/E12.png)
 
 ![_config.yml]({{ site.baseurl }}/images/EBots/E13.png)

This is because we have not activated Einstein Platform services which is resulting in the Failure of Bot training as shown above. Once you activate the Bots you must have an email from Einstein Platform services as shown in the image below:
![_config.yml]({{ site.baseurl }}/images/EBots/E14.png)

Select Production and link your Salesforce Einstein Platform Services account to a certificate in your Salesforce org. If you don't have a certificate yet, from Setup, enter Security in the Quick Find box, then select Security |
Certificate and Key Management and click Create Self-Signed Certificate.

Now lets go the overview page and train our Bobby Axelrod Bot. You now see the status changing from queued to running which means its working. 
![_config.yml]({{ site.baseurl }}/images/EBots/E15.png)

Once the training is completed you will receive a notification you can save and preview your bot as shown:

![_config.yml]({{ site.baseurl }}/images/EBots/E16.png)

## Preview:
![_config.yml]({{ site.baseurl }}/images/EBots/EG1.gif)

I have also created a Force.com site with Live agent to start the Chat externally. 
![_config.yml]({{ site.baseurl }}/images/EBots/EG2.gif)

Its been never so easy to create a chatbot, thanks to Salesforce for bringing Einstein Bots to us this Summer'18. Now its your turn to put Einstein Bots to work for your dear customers. 

## Resources:
1. [Help Article](https://help.salesforce.com/articleView?id=bots_service_web_chat.htm&type=5)
2. [Developer Blog](https://developer.salesforce.com/blogs/2018/06/summer18-einstein-bots-for-the-people.html)
