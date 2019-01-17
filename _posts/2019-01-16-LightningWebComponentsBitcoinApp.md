---
layout: post
title:  Building Bitcoin Price App with Lightning Web Components 
---
![_config.yml]({{ site.baseurl }}/images/lwcbtc/btc.jpg)

## Frameworks - repetition is constant 

As a developer, we often fall in love with one framework until a next batter one arrives. Once the better one arrives, we start learning and building new apps with new frameworks. The evolution of frameworks is constant and so is their learning curve. Even though it is advantageous, it takes a lifetime to give constant love to a single framework like Lightning, React, Angular etc. We always need to rinse, wash and repeat ;) Frameworks pull us in a million different directions and train us to do things from the framework author’s perspective. These practices might be good, bad, or completely new concepts. 

## Web Components Era 

Everyone here must have used form tag,  select tag etc and build pages out of these tags. They had encapsulation, they had default UI, they would emit events when something interesting happened. The way we build pages on the web these days, we either copy & paste chunks of HTML from CSS libraries like Bootstrap or litter our pages with all sorts of JavaScript frameworks and plugins. On top of that, reusing components from different frameworks in the same page isn't always possible. This means our pages end up with bloated CSS, bloated JavaScript or both. What if HTML was expressive enough to allow us to extend HTML so we can fill in the gaps in functionality with our own tags? Well, Web Components enable that. W3C specifications that make Web Components are below:

1. Custom Elements lets you define your own HTML tags;
1. HTML Templates enables you to define blocks of markup with the ability to inject dynamic content into;
1. Shadow DOM gives you the ability to scope markup and styles in a separate DOM tree;
1. HTML Imports provides a way to include and reuse HTML documents in other HTML documents.

Mozilla defines Web Components as a suite of different technologies allowing you to create reusable custom elements — with their functionality encapsulated away from the rest of your code — and utilize them in your web apps.  

Lightning Web Components is the Salesforce implementation of that new breed of lightweight frameworks built on web standards. It leverages custom elements, templates, shadow DOM, decorators, modules, and other new language constructs available in ECMAScript 7 and beyond.In simple terms, LWC is a reusable custom HTML element with its own API. You can get started with creating the first Lightning Web Component by blazing this [trail](https://trailhead.salesforce.com/en/content/learn/projects/quick-start-lightning-web-components). Once the basic LWC setup is ready, you can proceed with the blog post to build a fun Bitcoin Price now Lightning Web Component.

LWC doesn’t support bi-directional data binding and requires to bind the input field value to the property and register an onChange event listener that updates the value of the declared property on user action.

## Using Lightning Data Service
If you are a trailblazer who prefers using cool LDS functions for single records over Apex calls, below are few approaches to utilize LDS in LWC:
* lightning-record-form: It is the fastest/most productive way to build a form.
* lightning-record-view-form or lightning-record-edit-form: This your way if you need more control over the layout, want to handle events on individual input fields, or need to execute pre-submission.
* @wire(getRecord): Your choice for more control over the UI or access data without a UI.
Static schema and dynamic schema are two ways to implement record forms. Static schema ensures referential integrity while dynamic schema supports agnostic components endorsing loose coupling between Model & View.

## Calling Apex from LWC
In order to work with Apex from LWC the Apex methods must be imported. Once imported, you can invoke an Apex method using two different approaches:
* Imperative Apex: You call the imported method like any other asynchronous function in JavaScript. The function call returns a promise.
* Using @wire: You use the @wire decorator to wire the imported method to a component property or function. The wired method is automatically invoked when parameter values are initialized and is invoked again whenever parameter values change.

Salesforce suggests using @wire over imperative Apex method unless you have scenarios that require responsive actions like buttonClick etc.

As a quick hands-on exercise, let’s build an application that utilizes imperative Apex on a button click. In this application, we will make an API call from the Apex class to blockchain.info API to get the current BTC price. To expose an Apex method to a Lightning web component, the method must be static and either global or public. Annotate the method with @AuraEnabled. To improve runtime performance, annotate the Apex method with @AuraEnabled(cacheable=true), which caches the method results on the client. To set cacheable=true, a method must only get data, it can’t mutate (change) data. Marking a method as cacheable improves your component’s performance by quickly showing cached data from client-side storage without waiting for a server trip. If the cached data is stale, the framework retrieves the latest data from the server. Caching is especially beneficial for users on high latency, slow, or unreliable connections. The caching refresh time is automatically configured in Lightning Experience which is typically duration in seconds. Here is the Apex class that makes the REST call and returns BTC price as a string.

```
// sfdcBtcPrice.cls
public with sharing class sfdcBtcPrice {
    @AuraEnabled(cacheable = true)
    public static String getBTCPrice() {
        string url = 'https://blockchain.info/tobtc?currency=USD&value=1';
        Http h = new Http();
        HttpRequest req = new HttpRequest();
        req.setEndpoint(url);
        req.setMethod('GET');

        HttpResponse res = h.send(req);
        String BTCprice = (string)(res.getBody());
        return BTCprice;
    }
}

```

To get an Apex class in action we need to import in the LWC JavaScript source file as below:

```
import apexMethod from '@salesforce/apex/Namespace.Classname.apexMethod';

```
In our case, to get our Apex class in action we need to add the below code to the Javascript source file of LWC.

```
//sfdcBtcPrice.js
import { LightningElement, track } from 'lwc';
import getContactList from '@salesforce/apex/sfdcBtcPrice.getBTCPrice';

export default class ApexImperativeMethod extends LightningElement {
    @track BTCPrice;
    @track error;

    handleLoad() {
        getBTCPrice()
            .then(result => {
                this.BTCPrice = result;
            })
            .catch(error => {
                this.error = error;
            });
    }
}


```

As this is an imperative Apex approach, we are not actually using @wire but we are still using @track. It lets you re-render the value in the template whenever an action occurs.

Finally, the HTML template of the LWC uses the if:true directive to check for the JavaScript BTCPrice property. If the error property exists, the component renders c-error-panel tag.


```
< !— sfdcBtcPrice.html —>
    <template>
        <lightning-card title="Bitcoin Price Now App" icon-name="custom:custom17">
            <div class="slds-m-around_medium">
                <p class="slds-m-bottom_small">
                    <lightning-button label="Get BTC Price" onclick={handleLoad}></lightning-button>
                </p>
                <template if:true={BTCPrice}>
                    1 USD = {BTCP rice} BTC
                </template>
                <template if:true={error}>
                    <c-error-pane l errors={error}>
                        </c-error-panel>
                </template>
            </div>
        </lightning-card>
    </template>


```

Here is the demo of the working component:
![_config.yml]({{ site.baseurl }}/images/lwcbtc/btc.gif)

Also to make sure the Bitcoin Price Now LWC is available or all record, app and homepages your metadata file needs to tweaked as below:

```
<!— sfdcBtcPrice.js-meta.xml —>
<?xml version="1.0" encoding="UTF-8"?>
<LightningComponentBundle xmlns="http://soap.sforce.com/2006/04/metadata" fqn=“sfdcBtcPrice”>
    <apiVersion>45.0</apiVersion>
    <isExposed>true</isExposed>
    <targets>
        <target>lightning__AppPage</target>
        <target>lightning__RecordPage</target>
        <target>lightning__HomePage</target>
    </targets>
</LightningComponentBundle>

```
## Resources:
1. [Developer Guide](https://developer.salesforce.com/docs/component-library/documentation/lwc/apex)
2. [Developer Blog](https://developer.salesforce.com/blogs/2018/12/introducing-lightning-web-components-recipes-patterns-and-best-practices.html)
3. [Amit Salesforce](http://amitsalesforce.blogspot.com/2018/12/lightning-web-components-lwc-Salesforce.html)
