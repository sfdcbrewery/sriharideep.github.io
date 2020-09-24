---
layout: post
title:  Dynamic HTML and CSS rendering in Lightning Web Components (LWC) for Desktop and Mobile 
---
![_config.yml]({{ site.baseurl }}/images/webmob/dyn.png)

LWC has different lifecycle hooks which are essentially  are callback methods triggered at a specific phase of a component instance’s lifecycle.

We are interested in the render() callback method to update the UI conditionally. Render() can be called before or after connectedCallback(). Though it is rare to call the render() in a component it's main use case is to conditionally render a valid HTML template based on business logic.

For example, imagine that you have a component that can be rendered in two different ways but you don’t want to mix the HTML in one file. Create multiple HTML files in the component bundle. Import them both and add a condition in the render() method to return the correct template depending on the component’s state.

Following the lines of code splitting used in some JavaScript frameworks, one can import multiple HTML templates and write business logic that renders them conditionally. Create multiple HTML files one for web and one for mobile in the component bundle. Import them all and add a condition in the render() method to return the correct template depending on the device the component is accessed. The returned value from the render() method must be a template reference, which is the imported default export from an HTML file.

```
// insidersMultipleTemplates.html

<template>
    
</template>

```

```
// insidersMultipleTemplates.js

import { LightningElement } from 'lwc';
import insidersMobileTemplate from './insidersMobileTemplate.html';
import insidersWebTemplate from './insidersWebTemplate.html';

export default class RenderHtmlConditionally extends LightningElement {
    render() {
        return window.screen.width < 768 ? insidersMobileTemplate : insidersWebTemplate;
    }
}
```

```
// insidersMultipleTemplates.*js-meta.xml*
<?xml version="1.0" encoding="UTF-8"?>
<LightningComponentBundle xmlns="http://soap.sforce.com/2006/04/metadata" fqn="renderHtml">
    <apiVersion>46.0</apiVersion>
    <isExposed>true</isExposed>
    <targets>
      <target>lightning__RecordPage</target>
      <target>lightning__AppPage</target>
      <target>lightning__HomePage</target>
      <target>lightningCommunity__Page</target>
      <target>lightningCommunity__Default</target>
    </targets>
</LightningComponentBundle>

```

```
// insidersMobileTemplate.html

<template>
    <lightning-card variant="Narrow" title="Mobile Template" icon-name="custom:custom11">
        <lightning-button-icon icon-name="utility:down" variant="border-filled" alternative-text="Show More"
            slot="actions"></lightning-button-icon>
        <p class="slds-p-horizontal_small">This is Mobile template.</p>
    </lightning-card>
</template>

```

```
// insidersWebTemplate.html

<template>
    <lightning-card variant="Narrow" title="Web Template" icon-name="custom:custom33">
        <lightning-button-icon icon-name="utility:down" variant="border-filled" alternative-text="Show More"
            slot="actions"></lightning-button-icon>
        <p class="slds-p-horizontal_small">This is Web template.</p>
    </lightning-card>
</template>

```

To reference CSS from an extra template, the CSS filename must match the filename of the extra template. For example, insidersMobileTemplate.html can reference CSS only from insidersMobileTemplate.css. It can’t reference CSS from insidersMultipleTemplates.css or insidersWebTemplate.css.

```
insidersMultipleTemplates
   ├──insidersMultipleTemplates.html
   ├──insidersMultipleTemplates.js
   ├──insidersMultipleTemplates.js-meta.xml
   ├──insidersMultipleTemplates.css
   ├──insidersMobileTemplate.html
   ├──insidersMobileTemplate.css
   ├──insidersWebTemplate.html
   └──insidersWebTemplate.css

```

We can also use an if:true-false directive to render nested templates conditionally instead. 

Once we deploy the LWC components you can see the dynamic behavior based on the screen size.

# Component in Web:
![_config.yml]({{ site.baseurl }}/images/webmob/web.png)

# Component in Mobile:
![_config.yml]({{ site.baseurl }}/images/webmob/mob.jpeg)

# Resources:
1. [Salesforce Dev Docs](https://developer.salesforce.com/docs/component-library/documentation/en/lwc/lwc.create_render)
2. [Salesforce Diaries](https://salesforcediaries.com/2019/09/29/conditional-rendering-of-different-html-file-in-lightning-web-component/)