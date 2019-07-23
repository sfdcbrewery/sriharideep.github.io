---
layout: post
title:  Coding for web accessibility on Salesforce 
---
![_config.yml]({{ site.baseurl }}/images/ui.png)

Wikipedia defines *Web accessibility* as the inclusive practice of ensuring there are no barriers that prevent interaction with, or access to, websites on the World Wide Web by people with disabilities. When sites are correctly designed, developed and edited, generally all users have equal access to information and functionality.

Web Content Accessibility Guidelines (WCAG)  by  a project by the World Wide Web Consortium (W3C*)* is a set of guidelines, composed and reviewed by a global community of digital experts, that make digital content accessible for users with disabilities. The WCAG guidelines have four principles. 

* *Perceivable*. Users must be able to perceive the information being presented.
* *Operable*. Users must be able to operate the interface. The interface cannot require interaction that a user cannot perform.
* *Understandable*. Users must be able to understand the information and the operation of the user interface. The content or operation cannot be beyond the user’s understanding.
* *Robust*. Users must be able to access the content using a wide variety of user agents, including assistive technologies.

Avoiding complex user interfaces in an application is the key to building accessible web page or application.Semantic markup helps to develop content that appears on a web page should be presented using markup that represents the type of content that is being presented.   For example, a blind user can use a screen reader to navigate a report builder created with properly formed HTML <table> elements. However, this same user can’t properly navigate or understand the same thing marked up using <div> elements, even though they may look the same to sighted users. Hence HTML structure of web pages and applications add content meaning to assistive technologies, and users who rely on assistive technologies.

## ARIA(Accessible Rich Internet Applications)

WAI-ARIA(Web Acessbility Intiative - Accessible Rich Internet Applications) is a technical specification that provides a framework to improve the accessibility and interoperability of web content and applications. ARIA works by applying special attributes to HTML DOM nodes. There are three types of attributes available in ARIA: roles, states, and properties.

1. Roles- Roles are a way to give semantic meaning to HTML elements that traditionally do not have any semantic meaning, such as <div> or <span>. For instance, you can use ARIA roles to identify a set of non-semantic elements as a button or a link, or even more complex components such as menus, comboboxes, modals, or interactive grids. 

2. States - States are attributes that describe the status of an ARIA component to a browser’s accessibility tree. A state describes whether or not a dropdown menu is expanded, whether its input type is disabled or readonly, if a checkbox is checked, if an item in a listbox is selected, and so on. 

3. Properties - More like attributes that are essential to the nature of a given object, or that represent a data value associated with the object. "<select>" element can have multiple selections vs only one selection. 

 [Salesforce Lightning Design System](https://www.lightningdesignsystem.com/) (SLDS). SLDS contains over 900 blueprints of HTML, with detailed accessibility guidelines for markup, attributes, and keyboard interactions. 

Context and focus management is key to building accessible apps. Consider moving a user’s focus or using an aria-live region appropriately. Refer  [SLDS Global Focus Guidelines](https://www.lightningdesignsystem.com/accessibility/guidelines/global-focus/) for more details. 

In Salesforce world, we have Aura and Lightning components. Aura component include different namespaces like ui, aura etc. and have older ARIA specification which no longer meet the ARIA specification. Aura components in the lightning namespace, as well as Lightning web components with the lightning- prefix, are all written to the latest ARIA standard and follow the latest in SLDS component blueprints. Wherever possible, use these components over their older alternatives. Many Lightning components also have attributes to set ARIA properties.

Here is a not very accessible component

```
<ul>
  <lightning-example-list-item>
    <li>content</li>
  </lightning-example-list-item>
  <lightning-example-list-item>
    <li>content</li>
  </lightning-example-list-item>
  <lightning-example-list-item>
    <li>content</li>
  </lightning-example-list-item>
</ul>
```

Now here is an accessible component

```
<lightning-example-list>
<div role="list">
  <lightning-example-list-item>
    <div role="listitem">content</div>
  </lightning-example-list-item>
  <lightning-example-list-item”>
    <div role="listitem">content</div>
  </lightning-example-list-item>
  <lightning-example-list-item>
    <div role="listitem">content</div>
  </lightning-example-list-item>
</div>
</lightning-example-list>
```

You need to be. careful about the Shadow DOM. Many aspects of web accessibility rely on component ID references linking different elements on the page. A simple example of this involves labeling form inputs.

Non accessbile way:

```
<lightning-example-label>
     <label for="foo">First Name</label>
</lightning-example-label>
<lightning-example-input>
     <input type="text" id="foo" />
</lightning-example-input>

```
Accessbile way:

```
<lightning-example-input>
     <label for="foo">First Name</label>
     <input type="text" id="foo">
</lightning-example-input>

```

Keeping them in one Shadow DOM preserves the relationship between input and label.

You can call Aura accessibility tests in using the below two environments.

1. JSTEST -> $A.test.assertAccessible() 
2. WebDriver Test -> auraTestingUtil.assertAccessible() 

If you enjoyed reading this blog post, don’t wait to lead the accessibility trail by completing the (Web Accessibility trail)[https://trailhead.salesforce.com/content/learn/trails/get-started-with-web-accessibility]. 


## “Abled does not mean enabled. Disabled does not mean less abled.”

# Resources:

1. [Accessibility Trail]([https://trailhead.salesforce.com/content/learn/trails/get-started-with-web-accessibility)
2. [W3C](https://www.w3.org/TR/wai-aria-1.1/)

