---
title: First Ruboto Release!
layout: post
---

I finally have a first release of my [Ruby Summer of Code](http://rubysoc.org/) project, [Ruboto](http://ruboto.org/). 

Ruboto is Ruby for Android--the goal is to allow programmers to do everything they could do by writing a Java Android app in Ruby, writing no Java code at all (and hopefully no XML either). 

It doesn't quite fit that description yet. You can write Activities, BroadcastReceivers, and Services in Ruby. That's all it supports so far, but it should be pretty easy to add other classes. And I haven't yet tackled the issue of all of that ugly XML, but that's coming soon.

It's super-alpha. I'll be shocked if there aren't bugs, and I can almost guarantee you that 0.0.2 will not be backwards compatible. So for now, treat this as something to play around with, not something to write production apps with.

Enough boring you. To install it just run

    $ gem install ruboto-core

Right now, the only docs are the [README](http://github.com/ruboto/ruboto-core#readme), but I've made it pretty comprehensive (and long ...). 

A few more random things:

* If you're at a loss for how to write scripts, the [demos](http://github.com/ruboto/ruboto-irb/tree/master/assets/demo-scripts/) are really helpful.
* All ruboto scripts written for the [irb](http://github.com/ruboto/ruboto-irb) should work. If you find that not to be the case, please let me know.
* It's slow. Really slow. Once the app gets started, it's fine, but it takes forever for JRuby to get setup because Dalvik's reflection libraries are slow. They're going to fix that eventually, but we'll probably work around it before then.

# Next

So this is 0.0.1. What's coming up?

Better docs. 

Better error messages. 

Plugins. So that people can encapsulate common patterns and make scripts simpler. 

Easier configuration. XML sucks. That'll be a little tough because I need to preserve the default configuration that Android generates, and I want plugins to be able to generate configuration too. But I'll figure it out. 

And then I'll write a couple sample apps. 

That should all happen in the next week and a half before RSoC is over. After then, there's plenty to do like supporting the rest of the classes, speeding it up, making it perfect, etc. 
