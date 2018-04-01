---
layout: post
title:  Mine Monero on Heroku using Node.js  
---
![_config.yml]({{ site.baseurl }}/images/monero/Monero.jpg)

##Note: SFDC Brewery or Sri Kolagani does not endorse or support Monero & mining Monero on Heroku. This is just for demo purposes. 

#What is Monero?
Monero is a secure, private, untraceable currency that is open-source and freely available to all. You are your own bank, you control and are responsible for your funds, and nobody can trace your transfers. For more information visit Monero official [website](https://getmonero.org/)

# SFDC Brewery Monero miner

1) Fork or Clone this Repository

2) Run "npm start"

3) Create a (free) account on https://coin-hive.com/ and get your API Key under Settings / Sites & API Keys  (ex. OwZyZ3HVKjMeMTrodeXC2iZ7ZGY8eOdT)

4) Replace the API key in index.js (line 7) with your API Key

5) Create a (free) account on http://www.heroku.com

6) On Heroku create a new application

![alt text](/images/monero/1.png)

7) Add the following buildpacks 
- heroku/nodejs
- https://github.com/jontewks/puppeteer-heroku-buildpack.git

![alt text](/images/monero/2.png)
![alt text](/images/monero/3.png)

8) Connect your Heroku app to your GitHub repo and enable the automatic deployment feature

![alt text](/images/monero/4.png)

9) Verify that the solution builds 

![alt text](/images/monero/5.png)

10) The solution is basically a web server that does nothing but behind the scenes is using the heroku server to mine monero

![alt text](/images/monero/6.png)

11) Login to your coin-hive account and verify you are receiving reward
-----------------
Note: After 30 minutes the heroku instance will go to sleep so be sure to sign up for another free service (https://uptimerobot.com/) which can ping your url every 5 minutes to make sure it stays up.

# HAPPY MINING! 

Get the full code [Git repo](https://github.com/sfdcbrewery/MoneroHerokuApp)

#Warning: According to few responses, we observed Heroku suspending the accounts for mining inside it. So, try it on a Heroku demo org.

If you like this article, consider reading:
1. [Monero White Paper](https://github.com/monero-project/research-lab/blob/master/whitepaper/whitepaper.pdf)
1. [Heroku Developement Center](https://devcenter.heroku.com/categories/reference)