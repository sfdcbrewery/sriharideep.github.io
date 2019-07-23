---
layout: post
title:  Know your Pokémon with LWC Open Source Components and Heroku 
---
![_config.yml]({{ site.baseurl }}/images/poke.png)

Lightning Web Components framework was open sourced in TrailHeaDX 2019, allowing developers to contribute to the roadmap and to use the same framework whether they are building applications on Salesforce or on any other platforms like Heroku, AWS etc. In the past, developers often had to use different frameworks to build different sides of an application. For example, you’d use Aura to build the employee-facing side of an application on Salesforce and React, Angular or Vue to build the customer engagement side of the application on Heroku or any other platform. Today, you can use Lightning Web Components to build both sides of the application. The benefits are significant: you only need to learn a single framework and you can share code between apps. 

![_config.yml]({{ site.baseurl }}/images/poke/4.png)


Lightning Web Components was born as a modern framework built on the modern web stack. Among other standards, it leverages custom elements, templates, decorators, modules, and other new language constructs available in ECMAScript 6 and beyond.

Lightning Web Components has three key elements:

1. The Lightning Web Components framework: the engine of the framework.
2. The Base Lightning Components: 70+ UI components all built as custom elements.
3. Salesforce Bindings: A set of specialized services that provide declarative and imperative  access to Salesforce data and metadata, data caching, and data synchronization.

Lightning Web Components framework doesn’t have dependencies on the Salesforce platform hence giving the flexibility to build apps on any platform with our favorite Lightning touch.

In this blog post, lets build a know your Pokémon app that provides the exclusive skills search of your favorite Pokémon. We will be using (PokeAPI)[https://pokeapi.co/], LWC OSS and Heroku to build this app. One my favorite features of LWC is the local development which lets you view the project on localhost before deploying it to cloud. 

Lets get started by cloning the app and use the commands below:

```
git clone https://github.com/sfdcbrewery/Pok-monLightningWebComponent/

cd Pok-monLightningWebComponent

npm install 

npm run watch 

```
Now you should be able to see the app in your local host  once the build is successful. 

![_config.yml]({{ site.baseurl }}/images/poke/1.png)

![_config.yml]({{ site.baseurl }}/images/poke/2.png)

Going into the details, we are using Fetch API to call the PokeAPI from our LWC component as shown in the code below:

```
knowyourpokemon.html

<template>
    <ui-card title="Gotta Catch 'Em All - Search Pokémon powers">
        <div class="container">
            <img src="/resources/images/pokemon.png" x="0" y="0" height="150px" width="250px" />
            <div class="search">
                <ui-input type="search" onchange={handleSearchKeyChange} label="Type Pokémon Name" value={searchKey}>
                </ui-input>
                <p class="margin-vertical-small">
                    <ui-button label="Search" class="button" onclick={handleSearchClick}></ui-button>
                </p>
            </div>
            <template if:true={pokemons}>
                <ul style="list-style-type:square;" >
                    <template for:each={pokemons.abilities} for:item="pokemon">
                        <li key={pokemon.id} class="search-results">
                            {pokemon.ability.name}
                        </li>
                    </template>
                </ul>
            </template>
            <template if:false={pokemons}>
                <template if:true={error}>
                    <recipe-error-panel errors={error}></recipe-error-panel>
                </template>
            </template>
        </div>

        <recipe-view-source source="recipe/" slot="footer">
            Inspired from René Winkelmeyer LWC OSS recipies&nbsp; </br>
            <a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API" target="_blank">Learn about Fetch API</a>.
        </recipe-view-source>
    </ui-card>
</template>

```
On click of search we make the call to the PokeAPI endpoint using the Js below:

```
knowyourpokemon.js

import { LightningElement, track } from 'lwc';

const QUERY_URL =
    'https://pokeapi.co/api/v2/pokemon/';

export default class MiscRestCall extends LightningElement {
    @track searchKey = 'pikachu';
    @track pokemons;
    @track pokeability;
    @track error;
    
    
    handleSearchKeyChange(event) {
        this.searchKey = event.target.value.toLowerCase();
    }

    handleSearchClick() {
        fetch(QUERY_URL + this.searchKey)
            .then(response => {
                if (!response.ok) {
                    this.error = response;
                }
                return response.json();
            })
            .then(jsonResponse => {
                this.pokemons = jsonResponse;
            })
            .catch(error => {
                this.error = error;
                this.pokemons = undefined;
            });
    }
}


```

You can add the style using css associated with the LWC component as shown below:

```
knowyourpokemon.css

.search-results {
    list-style: none;
}

ul {
    padding: 0;
}

li {
    margin: 0;
}

.margin-vertical-small {
    margin: 0 4 0 4;
}

/* Small hack to format the a href in the ui-card slot */
a {
    text-decoration: var(--text-decoration);
    color: var(--color-text-link);
}


```
We now refer the above LWC component in the main index.html of the app. Now once we have the LWC app ready, we can deploy it to Heroku using the below button in the repository:
[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

You can mention the environment type(Production, sandbox or testing) while deploying. 
You will love LWC more once your app gets deployed and you search the skills of your favorite Pokémon on Heroku App. 

Here is some live action:

![_config.yml]({{ site.baseurl }}/images/poke/poke.gif)

Here is the link to the [Heroku App](https://pokemonlwc.herokuapp.com/)

Special thanks to René Winkelmeyer for amazing LWC recipes.


## References:
1. [LWC recipes](https://github.com/trailheadapps/lwc-recipes-oss) 
1. [LWC Guide](https://lwc.dev/guide/)

Keywords: #SFDCBrewery #SriharideepKolagani #LWC #OpenSource #Salesforce #RemoteDevelopment #NodeJS #Heroku #Yarn #SalesforceCertification #NPM #LightningWebComponents #fetchAPI
