[HOME](../README.md)

---

# Javascript! ~ Getting over that midway hump!

Continuing on to Phase4; Javascript really started to open things up for me.  I had struggled during the Rails portion, not as much with grasping knowledge but more of a mid-way hump of the course and feeling discuraged.  So when javascript came around and we had to develop a single page application, I decided to have a little fun with it.  Loving the idea of making elements come to life on the page, I had a thought ... *Why not make Ruby as frustrated as its made me in the passed?*  So I did, and it definately cheered me up! 


## Plan of ATTACK!

So for this project, I started with the basic concept - `I have a Character, that I want to throw an Item at`.  So right away, I knew I needed a Character and Item model.  But in order to meet the project requirements, I had to have atleast one `has_many` relationship.  So you know what I thought ... Damn if I haven't been frustrated with all the error codes I've gotten in the passed - let's have my Character `have_many sayings`, creating my Sayings model.  With the basic concept in mind, I began on my journey.  I seeded some data, knowing that I wanted a few default charachters (Sinatra, Ruby, Java), some fun items (Pie, Bomb, shoe) and a few stock sayings.

It was hard for me to resist the urge to jump in and get to the design of it all - But I've learned from passed projects, it is not the way!  So I set out and created all my elements to have borders/padding and different colors.  It helped me visually identify my elements, as I continued to build my javascript files.


## Creating my Oject Class

I decided to use my Character for my Object Orientated Class, the thought was that as my item hits my character it will do damage changing that attribute each time. I ended up building everything off of this class and creating class methods to handle a lot of the DOM manipulation.  

*As my project progressed this ended up bigger than I though, but handled beautifully* 

## CRUD, DOM it!

So here was my dreaded portion, damn `.fetch()` requests. Sure, easy enough to GET data, but Ugh, some struggeles to Create/Update.  `debugger>json>serializer>reload` - REPEAT! It took me a good portion of time to fully grasp what was happening with the data coming back and how to retrieve it in the way I wanted.  Once I got this, I thought - Why would I want to keep doing this?? Just set this information to my object and call it a day.  Done!  I'm not doing a bunch of fetch request, but with the size of my app, I'm afforded to do that.  No doubt making api calls to other sites will prove a bit more challenging, but I definately grasped the concept of how my json data was being returned to me.   I used both FastJSON as well as `.to_json` methods.  Seeing both the values of each, but opting to go the FastJSON route with all the relational models.  After getting the data how I wanted (or atleast in a way I could retrieve it), it was not problem rendering the DOM.

## Events



(More to come...)