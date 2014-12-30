---
layout: posts
category: post
title: "The Mighty DavidBot"
---

### {{page.title}}

While at the the Iron Yard, our instructor, [David Rogers](http://about.me/al_the_x) (or @al_the\_x as he's referred to in some circles), had a very Socratic teaching method. Whenever we had a question for him that was obviously not well researched (and even some that were), he would reply with "What do YOU think the answer is?" or "Did you google that?". Although it was very frustrating at the time, it was the perfect approach to teach a group of aspiring developers where to find the answers. Instead of telling us the solution, he led us to the answer, at times he led us kicking and screaming but led us nonetheless.

After a particularly exciting lesson throughout the week on APIs, one of our weekend tasks was to make something interesting utilizing at least two public APIs. I teamed up with [Logan Arnett](http://loganarnett.com/) on this project and we got to brainstorming. We had often times joked about how we should make a robot that, when you ask it questions, will google the answer for you. It would be the perfect solution for our inevitable withdrawls after the class was over. With the answer right in front of us, we set out to build our very own DavidBot!

The first version was much more complex than we ended up with (a lesson learned... yet not soon enough, but thats a story for another post). The two APIs we originally wanted to use were the [WebSpeech API](http://www.google.com/intl/en/chrome/demos/speech.html) and the [HipChat API](http://hipchat.com/docs/apiv2/). Utilizing iFrames, we were going to create a page with the avatar of our fearless leader. You would simply ask the avatar a question, it would take that input and transcribe it using the WebSpeech API and then input the question into a ["Let Me Google That For You"](http://lmgtfy.com) search and return the results. Upon viewing the results, you can share any relevant links with you classmates by pressing the "Hipchat that shit" button. This would then pull the url of the iframe and drop it into hipchat from the newly created DavidBot account that [Susanna Miller](https://twitter.com/susannajmiller), our campus director, was kind enough to have created for us.

Developers with experience with iframes probably know where I'm going with this, but among many of the other problems with our approach, apparently iFrames URLs don't change despite the contents of the frame itself navigating away the original URL. Basically we were screwed. We couldn't pull the URL of the new page so if the classmates wanted to share the link, they would just share the URL to the LMGFTY search. On top that, Google does not play nice with iFrames and LMGTFY. When directed to the link, the animation would trigger and the question would be filled in the box, but upon clicking "search" it would not redirect to google.com. Upon doing research we learned all about cross-domain errors and issues.

Our idea was dwindling by the minute and I looked around for a solution. It was at that moment that I got an email and my chrome extension icon spun around to notify me. That's the answer! A chrome extension! But... I've never made one of those, what does it even entail? Well GOOD NEWS EVERYONE

<img src="http://img2.wikia.nocookie.net/__cb20090731021518/en.futurama/images/thumb/a/ad/GoodNewsEveryone.jpg/500px-GoodNewsEveryone.jpg" />

It's just HTML/CSS/JS!

Excited with the new shift, I contacted Logan and got to work. I salvaged what I could from the original idea, decided to scrap the voice command as it required more permissions from Chrome and I didn't have time to figure that out. A pivot midway through the weekend = some things will be cut. I did some research and began grokking what I could as fast I could. 

Turns out a chrome extension is just a tiny web app along with a manifest.json file. Here's ours:



		{
		  "manifest_version": 2,

		  "name": "David Bot",
		  "description": "Everyone needs a little al-the-x to help them",
		  "version": "1.0",

		  "permissions": [
		    "tabs",
		    "alarms",
		    "notifications",
		    "activeTab"
		  ],
		  "browser_action": {
		    "default_icon": "icon.png",
		    "default_popup": "extension.html"
		  }
		}


With just that, we were able to load up the extension in developer mode in the chrome browser. 

With one API down ([Chrome Extension API](https://developer.chrome.com/extensions/api_index)) and one more to go, I turned my focus to the hipchat API. This is where things get fun.


		document.addEventListener('DOMContentLoaded', function() {
		  var linkToShare = '';
		  var currentTab = document.getElementById('hipchat');
		  currentTab.addEventListener('click', function(){
		    event.preventDefault();
		      event.preventDefault();
		      chrome.tabs.query({currentWindow: true, active: true}, function(tabs){
		      $.ajax({
		        type: 'POST',
		        url: apiBase + auth_token,
		        data: JSON.stringify({
		          'message' : "'One of your classmates found this useful and wanted to share it with you' + " " + tabs[0].url",
		          'color'   : 'purple',
		          "message_format" : 'text'
		        }),
		        error: function(e){
		          console.log(e);
		        },
		        dataType: "json",
		        contentType: "application/json"
		        });
		      });
		    return false;
		  });
		});


it was my experience working with jQuery in a chrome extension that it was not very fond of utilizing the library at times. I had to mix in some traditional dom-traversal calls with the jquery to get it to work.

The hipchat API was not difficult to work with. Once I figured out how to post a message to a specific room, it was all about customizing the message. I decided on a canned "One of your classmates found this useful and wanted to share it with you" and added the link of the URL on the end.

And THUSLY, DavidBot WAS ALIVE!!

<img src="/images/DavidBot.png" />

I was very proud of this project and I was especially proud that I was able to make it quite useful in the end by replacing some of the logic and Javascript. I replaced the LMGTFY functionality and instead plucked the input in the text field and concatenated it to the front of the URL for the hipchat functionality. Now, if a student was searching around and came across an article their classmates might find interesting, they could type a description into the field and press the chat bubble. This would then drop the link into hipchat sharing it with everyone while not breaking your workflow.

<img src="/images/DBbrowser.png" />
<hr />
<img src="/images/DBchat.png" />

Does it solve some big world problem? No. But it's mine. I built it and I love it.

Long Live DavidBot.

[Here's the source code, if you're into that kind of thing](https://github.com/LoganArnett/RESTfull-Weekend/tree/master/extension)

