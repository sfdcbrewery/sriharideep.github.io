---
layout: post
title:  Lightning component framework event handling lifecycle
---
![_config.yml]({{ site.baseurl }}/images/LEH/leh.jpg)

The framework uses event-driven programming. You write handlers that respond to interface events as they occur. The events may or may not have been triggered by user interaction.In the Lightning Component framework, events are fired from JavaScript controller actions. Events can contain attributes that can be set before the event is fired and read when the event is handled.Events are declared by the aura:event tag in a .evt resource, and they can have one of two types: component or application.

## Component Events
A component event is fired from an instance of a component. A component event can be handled by the component that fired the event or by a component in the containment hierarchy that receives the event.

## Application Events
Application events follow a traditional publish-subscribe model. An application event is fired from an instance of a component. All components that provide a handler for the event are notified.

Always, try and use component events over application events for an effective containment hierarchy. Now WTH is containment hierarchy? It is a tree of components that has a top-level container as its root. 
Events are fired by user actions and events which are results of actions and are different from app and comp events mentioned above. There are also System events that is fired automatically by the framework during its lifecycle, such as during component initialization, change of an attribute value, and rendering. 

Assuming that you have some basic knowledge of Lightning events, lets Dig deeper into Component and application events. 
## Component Event Propagation
The framework supports capture and bubble phases for the propagation of component events similar to DOM handling patterns. This gives an opportunity for interested components to interact with an event and potentially control the behavior for subsequent handlers.

The component that fires an event is known as the source component. The framework allows you to handle the event in different phases. These phases give you flexibility for how to best process the event for your application.
Here is the phases and sequence of component event propagation:
1. Event fired—A component event is fired.
2. Capture phase—The framework executes the capture phase from the application root to the source component until all components are traversed. Any handling event can stop propagation by calling stopPropagation() on the event.
3. Bubble phase—The framework executes the bubble phase from the source component to the application root until all components are traversed or stopPropagation() is called.

Lightning component events also support both capture and bubble phases, but there are some framework specificities in terms of syntax and behavior. By default, component events are handled in the bubble phase, and that’s what most developers use.
To handle the capture phase, add a phase="capture" attribute to your event handler, like so:

```
<aura:handler name="cmpEvent" event="c:cmpEvent" action="{!c.handleCmpEvent}" phase="capture"/>

```

You two handlers to use both the phases and query the event’s current propagation phase by calling event.getPhase() in your event handling function which in-turn will either return capture or bubble. By default , in Lightning component hierarchy only parent components that create subcomponents (either in their markup or programmatically) can handle events which does not include container components.

Lets take the below example:
```
<!-- c:cmp1 -->
<aura:component>
    <c:cmp2>
        <c:cmp3>
    </c:cmp2>
</aura:component>

```

Here, an event transmitted by cmp3 can not be handled by cmp2 as its not the outermost component. However, cmp2 can handle it a sits the outermost component and includes cmp2 and cmp3. If you want a container component cmp2 to handle a component event, add an includeFacets="true" attribute to its handler, such as:

```
<!-- c:containerCmp -->
<aura:component>
    <aura:handler name=“cmp3” event=“c:cmp3” action="{!c.handleCmpEvent}" includeFacets="true"/>
    {!v.body}
</aura:component>


```

You can stop the event at any point by calling event.stopPropagation() regardless of the current propagation phase. This is not a best practice as other components won’t be able to handle the vents once stopped. Lightning Framework also gives us an option to resume and pause the event which can be a typical use case for this is asynchronous processing.

```
handleEvent:function(component, event, helper){
event.pause();
Var action = $component.get(“c.getABeer”);
action.setcallback(this, function(response){
if(response.getState() == “SUCCESS”) {
event.resume();
}  else if (state === "ERROR") {

      event.stopPropagation();
    }
});


}

```
![_config.yml]({{ site.baseurl }}/images/LEH/Example.gif)

Look at the example given in the documentation [here](https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/events_component_example.htm).

## Application Events 
Application events follow a traditional publish-subscribe model. An application event is fired from an instance of a component. All components that provide a handler for the event are notified. Here is the phases and sequence of application event propagation:
1. Event fired—An application event is fired. The component that fires the event is known as the source component.
2. Capture phase—The framework executes the capture phase from the application root to the source component until all components are traversed. Any handling event can stop propagation by calling stopPropagation() on the event.
3. Bubble phase—The framework executes the bubble phase from the source component to the application root until all components are traversed or stopPropagation() is called.
4. Default phase—The framework executes the default phase from the root node unless preventDefault() was called in the capture or bubble phases. If the event’s propagation wasn’t stopped in a previous phase, the root node defaults to the application root. If the event’s propagation was stopped in a previous phase, the root node is set to the component whose handler invoked event.stopPropagation().

Rest of the story remains the same as component events. 
You can find the example [here](https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/events_application_example.htm).

## Event Handling Lifecycle

The following chart from the developer documentation summarizes how the framework handles events:

![_config.yml]({{ site.baseurl }}/images/LEH/214.jpg)

## Resources:
1. [Documentation](https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/events_intro.htm)
2. [Blog](https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/events_intro.htm)