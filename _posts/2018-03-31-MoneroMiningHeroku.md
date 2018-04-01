---
layout: post
title:  Mine Monero on Heroku using Node.js  
---
![_config.yml]({{ site.baseurl }}/images/monero/Monero.jpg)

# SFDC Brewery Monero miner

1) Fork or Clone this Repository

2) Run "npm start"

3) Create a (free) account on https://coin-hive.com/ and get your API Key under Settings / Sites & API Keys  (ex. OwZyZ3HVKjMeMTrodeXC2iZ7ZGY8eOdT)

4) Replace the API key in index.js (line 7) with your API Key

5) Create a (free) account on http://www.heroku.com

6) On Heroku create a new application

![alt text](/images/1.png)

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

If you like this article, consider reading:
[Monero White Paper](https://github.com/monero-project/research-lab/blob/master/whitepaper/whitepaper.pdf)
[Heroku Developement Center](https://devcenter.heroku.com/categories/reference)