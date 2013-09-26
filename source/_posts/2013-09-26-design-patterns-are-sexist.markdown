---
layout: post
title: "Design Patterns are sexist"
date: 2013-09-26 16:39
comments: true
categories: [Design Patterns, Sexism, Ruby On Rails, Active Record, Refactoring]
---

Ok here we go: design patterns. One of those things that you hear the first time and you go "erm.. later." Then you hear them again, in the form of a very smart friend / developer telling you to read [Design Patterns: Elements of Reusable Object-Oriented Software](http://www.amazon.com/Design-Patterns-Elements-Object-Oriented-ebook/dp/B000SEIBB8) or any of the other books you'll find recommended on this Amazon page. You'll buy or steal it from the Internets, read a few pages, and move on with more practical stuff, feeling better about yourself. Maybe you'll even use one or two, either by re-inventing the wheel and feeling mighty proud about your work (duh), or by obeying to framework conventions (ActiveRecord). But then the day comes when... 

At a job interview yesterday I was asked to name some Design Patterns, and to explain them. Of course I know... F-bomb. MVC! Ok. Any more? Hm. Observer? I remember the name. F-bomb. Clearly, that puts me in the same category as a Non-FizzBuzzer right? Well, kind of. I think it's natural that the complexity of learning to program makes you not ask or answer questions you clearly have, in order to get to what your current problem is, and that's fine (YAGNI). But as soon as you either have enough head start to entertain the idea of cleaning up that past baggage you should. Otherwise, it'll come back to haunt you (actually, preferably in a job interview than in a clusterf-bomb design of an app that you have to maintain). 

So I'll now become an expert on Design Patterns, wicked. Actually retaining the info from this talk is the goal: 

<iframe width="560" height="315" src="//www.youtube.com/embed/5yX6ADjyqyE" frameborder="0" allowfullscreen></iframe>

[Here's the post](http://blog.codeclimate.com/blog/2012/10/17/7-ways-to-decompose-fat-activerecord-models/)

##And here's a problem:
Reading books makes me tired, and watching stuff is great but merely goes to my passive memory, the one that goes "Ah yeah I knew that (But couldn't tell you so I never used it in my code)". I learn stuff when 

- I explain it to people (or my right hand) 
- I remember it in a context that is either funny, embarrassing, or otherwise emotionally impacted me. 

##Solution:

- I'll (ab)use you for personal gain. Please use the comments to get even.
- I'll make it so it's embarrassingly sexist, with the probable repercussions to be shamefully remembered. 

![Model no.1](http://host3.images.cdn.fotopedia.com/4vlcmdk21v1b9-yihNBpYv-lw-medium.jpg "Yeah, Models (at least CC)")

## 1. Value Objects
Examples in Ruby: Fixnum, Float, URI, Pathname
If you end up creating state, and that state has some operations affiliated: Value Object! 
Why? You get domain specific comparisons... Oh f-bomb why is this so boring.
Better - Logic for stuff that has value, and can be compared, represented or transformed in more than one, or one non-trivial way should live in its own class. 
A BodyMassIndex value should probably know how to set itself up given the Model inquiring, compare itself to other eager Models, and maybe even translated into some sort of other horrible category to measure unnatural human conditions.  I can remember that. Next! 

## 2. Service Objects aka Strategy Pattern
Oh I love those. It fits modern consumption so much: I need something! Answer: take something, use it, throw it away.
My credit cards lets me know which services I use: car sharing, streaming music, screencasts, mobile phone, internet. All these have in common: I don't have to know how they work, I want to be able to switch them like underwear, yet they directly impact me. I just need an interface to them to tell them what I need, and expect to be served. Of course I could also attach what they do directly on the activity, DIY style. But then I have to deal with the complexity of each process. Why would I? I'd have to fire up all the different elements of the systems that power them, and connect them, every time I need stuff. Why? I'd have to make network calls myself, instead of just screaming "get!" - when I just want to drive a car. And I'd have to stick to exactly the way I'm streaming music every time I listen to it, or go through the pain of re-designing that process. Models definitely need Service Objects. 

![Model no.3](http://farm5.staticflickr.com/4122/4865924361_4c428ffe32.jpg "more models (still CC)")


## 3. Form Objects
So your client needs to enter stuff in a form for a shooting, and somehow, that stuff touches more than one (Active Record) Model. Chances are those Models are related, and have to be created in a certain fashion, order, or maybe even stuff has to be checked or modified in order to set things up correctly. That logic should go into its own class. Validate, create models, wire them up, hair, makeup, mix and match ethnicities for political correctness - and save! Sure looks cool in the controller, and you'll never again have to swap between [accepts_nested_attributes_for](http://api.rubyonrails.org/classes/ActiveRecord/NestedAttributes/ClassMethods.html), your browser and debugger/pry again to remember/understand why shit magically worked, and now doesn't anymore.

## 4. Query Objects
But we have Scopes or class methods for that! Why would we need a query object? For one, Models like to look pretty, queries don't. To the ugly class with you! Then you can set up your Query object to take a &block to execute on the return set - ha, and do the work! Which one of my really rich friends drives a really fast electrical engine car AND runs a non-profit? DudesToDate.new.find_each do |dude| self.make_available_to(dude) end #f-bomb sexism. I'll make up for it somewhere.

## 5. View Objects
Need stuff from a Model just for a view? Don't bother the Model with it, and don't put that stuff into the view or its helpers. Put it on a View Object that you pass the Active Record Model it needs to come up with the stuff we wanna show in the view. Her hair is gonna be different tomorrow anyways. 

![Model no.2](http://host3.images.cdn.fotopedia.com/flickr-8282291462-medium.jpg "more models (still CC)")


## 6. Policy Objects
Great for analytics: Some stuff needs to be checked on an instance to return a boolean. The ugly actual politics of that check are best not discussed socially, and don't need to bother the Model. Just let me know if that guy is a slut or not based on what we already know about him. If we need to query Facebook, that's actually a Service Object. If we want to return a result set of non-slutty guys, that's a Query Object, and will probably respond_to empty? with true.  

## 7. Decorator
Avoid callback magic confusion! Here's the trick: Give the Decorator class a reference to the Model instance, and then call stuff like "save" on the Decorator class, not the Model itself. Then have the Decorator add on stuff to "save", preferably what you think it would when you read the Decorator class name. New booking for that Model? Need to send her a reminder, book flight, hotel, bodyguard, all that other stuff? Well, she doesn't need to know how, does she?
Now you have sort of an explicit callback - "Do this add-on stuff along with this Model lifecycle event, in this specific case". Not the horrible "do this stuff every time even when I don't remember it does that and I find out hours later when shit already hit the fan" callback magic way.












  
