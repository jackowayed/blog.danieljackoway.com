---
layout: post
title: Install Redis On Joyent
---

I'm playing with Joyent's new Node.js Smart Machine in preparation for [Node Knockout](http://nodeknockout.com/)

I tried to install redis just by downloading, untarring, and making, but I ran into some issues. Here's how I got it to work. 

## Packages

I had to install a few things from the package manager to make this work. I installed a few things that I'm pretty sure are unnecessary, so I think all you should need to do is

    $ pkgin install gmake-3.81 gcc44-4.4.4

## Version

The first thing you should know is that [redis-node-client](http://github.com/fictorial/redis-node-client) requires at least redis 1.3.8. The 2 featured downloads on redis's Google code page were 1.2.6 and the rc for 2.0. So I just went for the rc. If you want to use something between 1.3.8 and 2.0rc4, go ahead, but this tutorial will assume you want 2.0rc4. 

## Install

Alright, time to install Redis. 

SSH into your Joyent box and run

    $ wget http://code.google.com/p/redis/downloads/detail?name=redis-2.0.0-rc4.tar.gz
    $ gtar -xzvf redis-2.0.0-rc4.tar.gz
    $ cd redis-2.0.0-rc4
    $ CC=`which gcc` gmake

And then it should work. `gmake test` failed, but it was an issue with the tests themselves, not the Redis install. So then I started a screen session (you'll have to install screen), ran

    $ ./redis-sever

in the screen session, switched out of screen, and ran

    $ ./redis-cli 
    redis> set testkey testval
    OK
    redis> get testkey
    "testval"
    redis> exit

So it at least works to a degree.

It doesn't look like Joyent will let me reset my Smart Machine to try that from scratch and make sure it works, so if anything doesn't work, or you know of a better way, comment and I'll update the post. 
