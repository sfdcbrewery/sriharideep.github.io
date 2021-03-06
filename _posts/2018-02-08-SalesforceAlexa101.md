---
layout: post
title:  Alexa 101
---
![_config.yml]({{ site.baseurl }}/images/Alexa/1/img3.jpg)

### Introduction to Alexa and Amazon Web Services

Voice interfaces are the next progression in a series of ever-adapting user interfaces that we use every day. With advancements in neural nets, natural language processing, and speech recognition, we have VUI (Voice User Interfaces) which is a next generation advancement to Graphical User Interface(GUI), web designs and touch-based interfaces. Alexa is the brain behind the Amazon Echo family of devices and other Alexa-enabled devices. Using Alexa is as simple as asking a question—just ask, and Alexa responds instantly. Alexa lives in the cloud and is always getting smarter.

To an Alexa enabled device, user makes a request by using a Wake word like Alexa, Amazon or Computer which will enable the blue light on the device to glow as an indicator that it is active and listening. The captured audio is called utterance and its streamed to the cloud. 

Once the utterance has been received in the cloud, a series of speech models are applied to it using automatic speech recognition (ASR) and natural language understanding (NLU) to figure out what you wanted and where to route that which is termed as Intent which can be as simple as finding weather, whats on calendar etc. Intents are registered by a skill that can handle the intents, and the skill provides a number of sample utterances to help Alexa map out where requests go.

Skills are built utilizing the Alexa Skills Kit, a collection of self-service APIs, tools, documentation, and code samples that makes it fast and easy for anyone to build for voice. Skills can be built using many different options such as AWS Lambda, Heroku, and custom web services communicated over HTTPS. As long as the skill is built to handle the incoming Alexa request in a secure manner, it doesn’t matter where it is hosted or what language it is written in.

The skill is then responsible for returning a response to Alexa. The response can contain text that is formatted to be spoken a certain way from Alexa or can even contain your own prerecorded audio files. The response can be more than just a voice response. The skill can also indicate that a card should be returned to the user. Cards can contain additional context via text plus an image that help supplement the voice response. The card is then accessible via the Amazon Alexa App, which is available on Fire OS, Android, iOS, and desktop web browsers. With the introduction of the Echo Show, there are even more advanced cards called display templates that can be returned to the user. Display templates provide more flexibility by supporting full-width images, text overlays, lists of images and text, and more.

Alexa can make your life easier and more fun by:

* Providing hands-free voice control for music and entertainment—“Alexa, play me some funky music.”
* Keeping an eye on the clock whether you’re cooking in the kitchen or snoozing in the bedroom—“Alexa, set a timer for 20 minutes.”
* Using your voice to manage shopping and to-do lists—“Alexa, add milk to my shopping list.”
* Helping you stay connected to the news that matters most to you—“Alexa, play my flash briefing.”
* Controlling smart-home devices such as lights, switches, thermostats, and more—“Alexa, set the bedroom to 72 degrees.”
* And [many more](https://www.amazon.com/b/ref=EchoCP_meet_alexa_pack?node=16067214011).

### Designing a VUI

Travel planner: “What are we doing for your next trip?”
You: “Well, I want to go hiking in Hawaii.”
Travel planner: “Sounds exciting! When do you want to go, and where are you leaving from?”
You: “Let’s plan for a month away; I’m leaving from Seattle.”

Using Alexa, building a conversational interface like this is a part of the design process that we want to explicitly design and plan. Understanding the Voice Design Process involves knowing the purpose of the skill. An important part of this process is to write for how people talk, not how they read and write. Visualize the two sides of the conversation, and read them out loud to see how they flow. Conversational UI consists of turns starting with a person saying something, followed by Alexa responding. This is a new form of interaction for many people, so make sure that you’re aware of the ways in which users participate in the conversation so that you can design for it. A great voice experience allows for the many ways people can express meaning and intent.

Before we go further, let’s get a quick introduction to the important terms and concepts in how users interact with Alexa. With your scripts created during the design process, you can begin to break down your expected utterances into intents.

![_config.yml]({{ site.baseurl }}/images/Alexa/1/img1.png)

Let’s quickly recap the key parts of this example.

* Wake word—this is the key for Alexa to start listening.
* Launch—a connector word to link the wake word and invocation name. Supported words include: ask, tell, open, launch, run, begin, and more.
* Invocation name—this is a custom phrase that customers say to invoke your skill. It is generally two–three words long and closely related to the skill’s functionality.
* Utterance—this is the specific phrase that the user wants to take action on with the skill.
* One-shot utterance—the top example is a one-shot utterance, where all information is given at once and fully satisfies what is needed to activate an intent.
* Slot value—a variable part of an utterance. In our travel-planning scenario, the starting location, destination, activity, and date are all slot values.
* Intent—an action that the skill can handle. A single intent can have many different utterances, with or without slots to account for what users may say.

Pro tip: Your skill doesn’t need to list out all possible utterances in order to capture the right intent, but the more examples that you have listed, the better the skill performs.

A response should be brief enough to say in one breath.Consider adding variety and a little bit of humor in with your responses. There are several methods to help interject a variety of pauses, emphasize words, whisper, and more, using Speech Synthesis Markup Language (SSML).

Alexa should also prompt users when needed and provide conversation markers like first, then, and finally. And Alexa should have responses for the unexpected—for example, when Alexa doesn’t hear or understand the user.

One fun activity to see if your voice design sounds natural is to role-play as Alexa. You need two other people to help with this. Have one person be your user, interacting with Alexa (you). Your job is to use your script and reply as Alexa. Have the other person act as a scribe to help note any responses that sound awkward and capture utterances from your user that you weren’t expecting.

### Skill types
![_config.yml]({{ site.baseurl }}/images/Alexa/1/img2.png)

The Alexa Skills Kit (ASK) is a collection of self-service APIs, tools, documentation, and code samples that makes it fast and easy for you to add skills to Alexa. By using ASK, you can leverage Amazon’s knowledge and pioneering work in the field of voice design. Learn, design, build and launch the skills.  There is a certification process to pass before your skill is listed in the Alexa Skills Store for anyone to discover and use. ASK includes tips for each type of skill to help you pass the certification process.

### Resources 
1. [Alexa Dialogue Design Detailed](https://www.amazon.com/clouddrive/share/PLKDyDip6Jv1HK450NTTGzJZJB4QjDyYxTMlQgmWDCQ?_encoding=UTF8&mgh=1&ref_=cd_ph_share_link_copy)
1. [Alexa Design Labs](https://alexa.design/labs-design)
1. [Alexa Design Video](https://www.youtube.com/watch?v=JwUxY2-kIbg)
1. [Developer Guide](https://developer.amazon.com/designing-for-voice/)
