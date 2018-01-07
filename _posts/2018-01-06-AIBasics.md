---
layout: post
title: Say hello to AI
---
![_config.yml]({{ site.baseurl }}/images/introtoAI/AI.jpg)

Aim of this blog post is to try and understand the basic terminology of AI world from Salesforce Developer point of view.

### What is Machine Learning?
“Machine learning provides computers with the ability to learn without being explicitly programmed”
We feed and train a mathematical model with large relevant inputs that can be the potential output to future inputs. 

### Introduction to Neural Networks
A neural network is a set of algorithms designed to recognize patterns. Neural networks are superficially based on how the brain works.It has a set of nodes arranged in multiple layers, with weighted interconnections between them. Each neuron combines a set of input values to produce an output value which is passed on hierarchal basis. Artificial neural networks what we call Deep-Learning.

### Deep Learning 
It is a wizard of multiple layers. At a basic level, the top layer trains on specific set of features and then sends that information to next layer and so on. Salesforce Prediction Vision Services uses deep learning for training and solving image recognition. 

### Natural Language Processing 
NLP is the ability to understand and interpret human languages and speeches. 

### Cognitive Computing
Cognitive computing involves self-learning systems that use data mining (big data), pattern recognition (machine learning), and natural language processing to mimic the way the human brain works. While AI helps the user with the decision post analysis, cognitive computing provides all the information necessary for user to decide.

### Pattern Recognition
Patterns are the set of objects/concepts that have a like existence. They can be similar in statistical and structural terms. 

### Data mining
Data mining is the process of finding patterns or correlations among dozens of fields in a relational database.

Lets start get a basic practical experience of the above mentioned terminology with a simple PoC leveraging Google API. 

Google API contains two categories mentioned below:
### Regression Model
Given a new item, predict a numeric value for that item, based on similar valued examples in its training data.

### Categorical Model
Given a new item, choose a category that describes it best, given a set of similar categorized items in its training data.

### Scenario for the PoC 
SFDC Brewery wants to find the probability of opportunity closure using their existing data from SFDC.

## How do we do it? 
Lets integrate Google Predictive API with Salesforce to predict the probability of the Salesforce opportunity closure based on the expected revenue and the opportunity type, which in turn can help to forecast revenue better.Let’s take an opportunity dataset from Salesforce, upload it into the Google storage, and build a regression type predictive model, train the model, and use it to predict the probability of the opportunity closure.

![_config.yml]({{ site.baseurl }}/images/introtoAI/Arch.jpg)

### Prerequisites
1. [Developer Account](https://developer.salesforce.com/signup)
1. [Google account](https://accounts.google.com/SignUp?hl=en)
1. [Enable Prediction API](https://accounts.google.com/SignUp?hl=en)
1. Create a Google Cloud Project and get the Project ID. 
1. [Get free Google Cloud Storage here](https://console.cloud.google.com/storage/browser)
1. Create a folder and then upload the data from [here]()
1. Open [Prediction API](https://developers.google.com/apis-explorer/#s/prediction/v1.6/).We will need to first enable OAuth for the project and use the right scope. The screenshot shows the OAuth 2.0 screen and scope enablement screen. We will use V1.6 in this demo project. You will need to select the auth/prediction checkbox as shown in the screenshot below.

![_config.yml]({{ site.baseurl }}/images/introtoAI/Auth.png)

### About the dataset 
The opportunity dataset can be generated from the Salesforce reports directly. You can use the csv from this [link]() for your convenience. The dataset header contains probability, opportunity-type and expected revenue; if you observe closely you can see that the probability is higher for the existing customers opportunity. 

### Next Steps 
Lets train the dataset leveraging Prediction API provided by google. The data is uploaded into the console manually. Post training, we will send a query from he Salesforce in a HTTP request. 
We will train the dataset using ###prediction.trainedmodels.insert API available. Do not not forget to enable the Prediction API in your Google Cloud Platform.

```
        {
          "modelType": "regression",
          "id": "opportunity-predictor",
          "storageDataLocation": 
          “###YOUR-FOLDER_NAME/SFOpportunity.csv"
        }

```
The response should be as shown below:
![_config.yml]({{ site.baseurl }}/images/introtoAI/Resp1.png)

You can get the status of the trained data using the ###prediction.trainedmodels.get API

Now to used this trained data to predict the probability on Salesforce side we write a Trigger on Opportunity object in SFDC. The goal of this trigger is to make an async call to the Prediction API whenever a new opportunity is inserted. 

Before we write the trigger, create a Field in Opportunity object of type Number(Length 5 & Decimals 2) with default settings. 

Start by creating a Apex class for parsing the JSON result from Prediction API as shown below:

```
public class PredictionAPIInput{
    public csvData input;
    public class csvData {
        public String[] csvInstance;
    }
}

```

Before creating a trigger on Opportunity lets add all the awesome functionality in an Apex that could be called from the Trigger as shown below:

```
//Apex Class to make a Callout to google Prediction API

public with sharing class opportunityTriggerHelper{

   @future(callout=true)
   public static void predictProbability(Id OpportunityId){
       
        Opportunity oppData = [Select Id,Amount,Type, Probability__c from Opportunity where Id =:OpportunityId];
       
        HttpRequest req = new HttpRequest();
        req.setEndpoint('callout:Google_Auth');
        req.setMethod('POST');
        req.setHeader('content-type','application/json');
    
        //Form the Body
        PredictionAPIInput apiInput = new PredictionAPIInput();
        PredictionAPIInput.csvData csvData = new PredictionAPIInput.csvData();
        csvData.csvInstance  = new list<String>{oppData.Type,String.valueof(oppData.Amount)};
        apiInput.input = csvData;
    
        Http http = new Http();
        req.setBody(JSON.serialize(apiInput));
        HTTPResponse res = http.send(req);
        System.debug(res.getBody());
        
        if(res.getStatusCode() == 200){
           Map<String, Object> result = (Map<String, Object>)JSON.deserializeUntyped(res.getBody());
           oppData.Probability__c = Decimal.valueof((string)result.get('outputValue'));
           update oppData;
        }
   }
}


```
The above Apex class offers HTTP request to the Google Prediction API using a future callout. 


Now add the below trigger on Opportunity object.

```
trigger opportunityPredictor on Opportunity (after insert) {
   if(trigger.isinsert && trigger.isAfter){
      OpportunityTriggerHelper.predictProbability
        (trigger.new[0].Id);
     }
 }

```

Now before we sail, lets authorize SFDC to call the Prediction API.
[Generate a client ID and client secret via the Google Auth screen](https://console.developers.google.com/apis/credentials). Go ahead and create a web application, do not forget to note down the Consumer secret and Consumer key which we will use in the SFDC.

Use Auth in Salesforce to set up authorization, as shown in the following screenshot. The path for it is SETUP | Security Controls | Auth. Providers | New | Choose Google and enter credentials

Once saved note down the callback URL and enter it in the Google Auth Screen as shown below:

![_config.yml]({{ site.baseurl }}/images/introtoAI/Authcallback.png)

Now lets create a named credentials to avoid passing auth token from Salesforce. Here the API that Apex class must hit:

```
https://www.googleapis.com/prediction/v1.6/projects/learned-maker-155103/trainedmodels/opportunity-predictor/predict

```

And here we go we see the probability as shown below:


Although this is a fun tutorial, it does come with few limitations which are mentioned below:

1. Prediction system dataset is too small to call it a model 
1. Periodic training of data is necessary 

Hope this tutorial provide you the required adrenaline rush to be the next Awesome AI geek of SFDC world. Keep watching future blog posts for more action.

![_config.yml]({{ site.baseurl }}/images/introtoAI/Prob.gif)

Get the complete code from [here.](https://github.com/sfdcbrewery/IntrotoAI-Einstein/tree/master/IntrotoAI-Einstein)

Thanks to Mohith Shrivastava, his book “Learning Salesforce Einstein” is an inspiration to this blog post. 


Keywords: #AI #Einstein # #EinsteinAnalytics #EinsteinTutorials #EinsteinCRM #Salesforce #Einstein #SFDCBrewery #SriharideepKolagani #SalesforceDX #SalesforceDevelopment #Salesforce #SFDCBrewery #Brewery #SalesforceOnlineTraining #SalesforceTutorials #SalesforceCertification #Sri #Kolagani
