---
layout: post
title:  Analyzing Salesforce Lightning Locker Service and Lightning Container Components 
---
![_config.yml]({{ site.baseurl }}/images/Locker/Lockerh.jpg)

## Introduction 
In this blog post, let’s discuss how to build secure applications using Lightning Component framework. Today, we can build application using Salesforce components, third-party components and even own custom components built from scratch. We are always tempted to use DOM manipulation third party libraries like jQuery to ease the development. Today Lightning Component Framework is abstract enough to take over DOM manipulation libraries like jQuery leading to more robust and maintainable code. We can also question ourselves using the UI libraries like Bootstrap which lack the Lightning identity hindering the Salesforce user experience. Lightning Component Framework offer out of the box tightly coupled components easing the whole application development process on Salesforce on par with MVC frameworks like AnglarJS and ReactJS. It is always important to avoid the security exploits caused by improper level of component isolation. Without the right level of isolation, a vulnerability in one component can provide access to sensitive data in other components and jeopardize the security of the entire system. To tackle this Lightning Component Framework provides two isolation mechanisms: LockerService and the Lightning Container Component.

## What is LockerService Isolation?
LockerService ensures a component can only traverse the DOM and access elements created by a component in the same namespace. This behavior prevents the anti-pattern of reaching into DOM elements owned by components in another namespace. Apart from the namespace based restrictions, Locker Service also enforces JavaScript ES5 strict mode and Content Security Policy (CSP) ensuring XSS free safe and best code.

Example:
```
<!—c:LockerService—>
<aura:component>
    <aura:attribute name="myval" default="10" type="Integer"/>
    <div id="myDiv" aura:id="div1">
        <p>SFDC Brewery Locker Service demo</p>
    </div>
    <lightning:slider step="10" aura:id="picklistPath" value="{!v.myval}" onchange="{!c.handleChange}"/>
</aura:component>
({ /* LockerServiceController.js */
    handleChange : function(cmp, event, helper) {
        console.log("cmp.getElements(): ", cmp.getElements());
        // access the DOM in c:LockerService
        console.log("div1: ", cmp.find("div1").getElement());
        console.log("button1: ", cmp.find("picklistPath"));
        // returns an error
        console.log("picklistPath element: ", cmp.find("picklistPath").getElement());
    }
})

```
We get an error as in the last line of the controller we are trying to Access the DOM element which is c namespace different from the slider thats in Lightning namespace.
![_config.yml]({{ site.baseurl }}/images/Locker/error.png)

## LockerService Advantages
1. Native DOM performance
2. Eliminates DOM scraping vulnerabilities
3. Cross-site scripting (XSS) and template injection are no longer possible

## LockerService Limitations
1. Non-compliant libraries will not work with LockerService

## Lightning Container Component
The Lightning Container Component(lightning:container) is a convenience component that wraps an iframe and provides a simple API to communicate between the iframe container and its content. Using <iframe>, we can embed HTML page that can be loaded in a different DOM enforcing own context and limited access to the parent DOM.

```
<aura:component access="global" implements="flexipage:availableForAllPageTypes">
    <lightning:container aura:id=“ReactApp“
                         src="{!$Resource.ReatcApp + ‘/home.html'}"
                         onmessage=“{!c.SayHiToLightning}”/>
</aura:component>

```
## Lightning Container Component Advantages
1. Isolation capability in the browser client side
2. Support for any third-party library, including ones that are not LockerService compliant.
3. Lightning Container Component Limitations
4. Limited communication mechanism between components (postMessage)
5. Components are constrained to a rectangle area on the page. Content may be clipped, rich interactions like drag-and-drop between components may not work, etc.
6. Heavier/Slower. If there are multiple iframes on a page, each iframe loads its own version of libraries.

## Conclusion 
As Lightning keeps maturing with the best industry practices, it is always a good developer practice to question the use too DOM manipulation libraries going forward. Locker Service should always be our first choice as it delivers performance and security without losing the native Lightning identity. You can always use lightning:container if your target library doesn’t support LockerService.

## Resources:
1. [Locker Service Developer Guide] (https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/security_code.htm)
2. [Lightning Container Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/container_overview.htm)
3. [Christophe Blog](https://developer.salesforce.com/blogs/2018/04/lightning-container-component-building-components-with-react-angular-and-other-libraries.html)