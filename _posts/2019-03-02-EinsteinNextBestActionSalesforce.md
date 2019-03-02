---
layout: post
title:  Einstein Next Best Action in action
---
![_config.yml]({{ site.baseurl }}/images/Enba/main.jpg)

For a business to be successful, its always important to guide its customers and employees in a right and successful path. Einstein Next Best Action is one such functionality that displays right recommendations to the right people at the right time. Create and display offers and actions for your users that are tailored to meet any unique business criteria. Develop a strategy that applies your business logic to refine those recommendations. Your strategy distills your recommendations into a few key suggestions that really can be of direct help to your customers, partners and employee trailblazers. 

All orgs receive 5,000 Next Best Action strategy requests per month at no charge. It's possible to track usage by navigating to Company Information from Setup. Einstein Next Best Action as a functionality relies on flows, recommendations, action strategies, and components and has standard objects for reporting. Below are few items you need to be aware of before building Einstein Next Best Actions:

1. Creating Flows: A recommendation object which is a standard object requires a reference to an action. When we say an action here, we are referring to an active screen flow. You can’t surface a recommendation if you don’t have any screen flows. Action field which acts a lookup to the screen flows in recommendation object, shows both active and inactive flows. An inactive flow changes to active when its referenced in any recommendation record. Manage Flow permission is required for creating and managing flows.
2. Creating Recommendations: Recommendations is a newly introduced standard object which has few standard fields like names, descriptions, acceptance labels, and rejection labels etc. Acceptance label is a field that specifies the label for the button that customers click to accept the recommendation while the rejection label is for the opposite. It is also an ideal practice to add an additional custom category field to the recommendation object as it gives you more control when loading, sorting, and filtering recommendations in Strategy Builder and more options when creating flows. Action field assigns the screen flow to the recommendation. Recommendations names are not unique, hence be careful when creating multiple recommendations with the same name. Manage Next Best Action Recommendations is required to manage recommendations.
![_config.yml]({{ site.baseurl }}/images/Enba/1.png)
3. Creating Action Strategies: At-least one recommendation with an active flow is required for an action strategy. Load elements require at-least one criteria.
4. Strategy Builder: It is a flow builder like point-and-click process automation tool used with Einstein Next Best Action. Strategy Builder funnels recommendation records through your business logic to determine which recommendations are surfaced on your record pages. Once we have our recommendations ready, they can be managed and customized in strategy builder. An action strategy is used to funnel the correct recommendations to the users. On the other hand, strategy connections lets you integrate external data and pull in information from other Salesforce systems to create robust strategies that utilize all your data. You can always save, test, troubleshoot and inspect any elements and data inside the strategy builder.

Here is the journey to create perfect recommendations and strategies for any use case:

![_config.yml]({{ site.baseurl }}/images/Enba/2.jpg)

It's time for a hands-on exercise.

Requirement: Display the next best action on account when the SLA is expired.

Desired output: Build a recommendation that suggests to reach out to support.

Below are the steps to achieve the desired output:

1. Create a flow: From the new flow builder, create a confirm screen that has checkbox to agree with the companies terms and conditions. Once the checkbox has been set to true, create a case with the subject SLA and show a screen with success text output.
![_config.yml]({{ site.baseurl }}/images/Enba/3.png)
2. Now lets create a recommendation record as shown below:
![_config.yml]({{ site.baseurl }}/images/Enba/4.png)
3. Now that our recommendation is ready, let's head to strategy builder to build the next best action. From the setup, navigate to Einstein Next Best Action. Choose new strategy on the Account object from the welcome screen.
![_config.yml]({{ site.baseurl }}/images/Enba/5.png)
Lets add a recommendation logic filter as shown below:
![_config.yml]({{ site.baseurl }}/images/Enba/6.png)
This logic checks if the Account record has a SLAExpirationDate in the past.
4. If the logic is true, lets trigger the recommendation using load with the criteria shown below:
![_config.yml]({{ site.baseurl }}/images/Enba/7.png)

Your final strategy should look like this:

![_config.yml]({{ site.baseurl }}/images/Enba/8.png)

Once you have it all setup, edit the account record page and add Einstein Next Best Action to the layout using Lightning App Builder. Here is the demo:

![_config.yml]({{ site.baseurl }}/images/Enba/demo.gif)


## Limitations:

1. Strategy Builder is only available in Lightning Experience.
2. It has no inbuilt AI mechanism.

Here is an interesting recommended session by MVP Daniel Peter and Amit Chaudhary on [Einstein Next Best Action](https://www.youtube.com/watch?v=oZMEkgCpX_0).


## Resources:
1. [MVP Daniel Peter and Amit Chaudhary](https://www.youtube.com/watch?v=oZMEkgCpX_0)
2. [Salesforce Help](https://help.salesforce.com/articleView?id=einstein_next_best_action.htm&type=5)
