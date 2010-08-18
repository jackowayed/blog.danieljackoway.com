---
layout: post
title: I Hate Python--What Don't I Understand?
---

I'm a Rubyist. A few months ago I dipped my toes into Python. And it was awful. I pretty much hated everything about it. 

But lots of people love Python. Really smart people. People I want to work with/for. So I really want to start liking Python. But as it is, I just can't do it. There must be some tricks for dealing with the issues that I've run into to make it more bearable, and I must not be fully understanding the awesomeness of Python. 

So I'm going to list all of my complaints about Python. Tell me how to deal with the issues I've encountered, and while you're at it, tell me about all of the awesomeness that I'm not realizing is there. 

# Syntax

Python's syntax is less friendly. I know that that's opinion to some extent, but I think it's pretty clear. It doesn't let me be as lazy as Ruby lets me, and I think it also often looks a lot worse. 

It doesn't let me omit parens, which I do all the time in Ruby, especially for methods without arguments. I think that usually looks better, and it allows me to lazy, which is pretty much always a plus. 

It also doesn't seem to be as flexible syntactically. Ruby's metaprogramming and nice syntax for passing blocks to methods allows for really beautiful DSLs like [Sinatra](http://sinatrarb.com/) and [Rake](http://rake.rubyforge.org/). 

And I think the order of things is a lot more natural when you do things the object-oriented Ruby way. `string.length` seems more natural to me than `len(string)`, though I guess that could be because I'm so used to Ruby. None the less, to me, the ordering that occurs to me feels backwards, just as Lisp's parentheses feel like they're in the wrong place. Then again, I've sort of gotten used to how Lisp looks, so maybe I'd get used to Python as well. 

# Whitespace Active

You had to know this was coming--it's really annoying that Python is whitespace-active. 

There's lots of reasons why I think that this is a bad idea. First of all, for really short scripts that I write in under a minute, sometimes I don't indent properly, and there's nothing wrong with that. I just want to get something working as fast as possible. 

Secondly, it makes the [irb](http://en.wikipedia.org/wiki/Interactive_Ruby_Shell) equivalent (executing the shell command `python` without any params) totally unusable. At the beginning of every single line that's in a block, I have to manually hit space however many times is necessary to get to the right level of indentation. That's ridiculous.

Strangely, I love [Haml](http://en.wikipedia.org/wiki/Haml). But Haml has a specific purpose--templating web pages. It doesn't need to be as flexible as my primary programming language needs to be. There's no interactive haml shell. If I am hacking together a website super-fast with Sinatra, sometimes I'll skip templating languages altogether. 

I think all of my complaints up to this point come down to control and trust. Python doesn't trust me to use good indentation practice on my own, so it forces me to. 

# Libraries

My experience with Python's standard library has been horrible. Just terrible. 

## urllib(2)

I was playing with the [Notifo API](http://api.notifo.com/). The simplest API in the world. Yet urllib and urllib2 made it a terrible experience. (Note that the [Notifo python libraries](http://github.com/notifo/Notifo-API-Libraries/tree/master/python/) did not exist at that time.)

All I needed to do was make an HTTP GET request and an HTTP POST request, both with some parameters and HTTP Basic Auth. 

Unfortunately, urllib2 thinks that this is a good API for doing HTTP Basic Auth: Here is the example from [urrlib2's docs](http://docs.python.org/library/urllib2.html):

{% highlight python %}
import urllib2
# Create an OpenerDirector with support for Basic HTTP Authentication...
auth_handler = urllib2.HTTPBasicAuthHandler()
auth_handler.add_password(realm='PDQ Application',
                          uri='https://mahler:8092/site-updates.py',
                          user='klem',
                          passwd='kadidd!ehopper')
opener = urllib2.build_opener(auth_handler)
# ...and install it globally so it can be used with urlopen.
urllib2.install_opener(opener)
urllib2.urlopen('http://www.example.com/login.html')
{% end %}

6 ugly lines to make an HTTP Basic Auth request? Um, Python, did you just tell me to go fuck myself? (see [http://browsertoolkit.com/fault-tolerance.png](http://browsertoolkit.com/fault-tolerance.png))

Also, what is this realm nonsense? I've made lots of HTTP requests with Ruby's libraries and never needed to know that realms existed. Why can't python's libraries abstract that too. 

For comparison, this is the example of how to use Net::HTTP, which is in Ruby's *core* library (no need to require it):

{% highlight ruby %}
res = Net::HTTP.post_form(URI.parse('http://username:pass@www.example.com/todo.cgi'),
                                        {'from'=>'2005-01-01', 'to'=>'2005-03-31'})
puts res.body
{% end %}

One line to make the request. No need to know what realm we're using. This is how things should be. If I used open-uri, which is in the standard library, I wouldn't even have to call `URI.parse`. 

## imaplib

I was going to complain about how imaplib is really low-level and is just an extremely thin wrapper that doesn't abstract the imap protocol away enough. For example, to move a message to Gmail's trash, you do this:

{% highlight python %}
server.store(integer_id, '+FLAGS', '[Gmail]/Trash')
{% end %}

Instead of say

{% highlight python %}
server.add_flags(integer_id, '[Gmail]/Trash')
{% end %}

or even get a message object when fetching messages from the server so that you could do

{% highlight python %}
message.add_flags('[Gmail]/Trash')
{% end %}

Unfortunately, I can't complain too much because Net::IMAP doesn't look much better. So I guess if you want a nice API, you have to jump up to a 3rd-party library. 

But I'll complain a little. When you do a search, it gives you the ids as a space-separated string (a string like "1 2 3"), and then when you want to pass several ids to it, you give a comma-separated string ("1,2,3"). I presume that this is how the IMAP protocol works, but it seems way natural to use arrays in both cases. The only advantage I see to this approach is a slight performance win. As it is, when you do something like this: 

{% highlight python %}
# get the ids of all unread messages
status, ids = server.search(None, '(UNSEEN)')
# should do something here if status != "OK"

# fetch the actual messages, rather than just their ids
status, messages = server.fetch(ids[0].replace(' ', ','), '(RFC822)')
{% end %}

you go straight from the space-separated string to the comma-separated one. If they both used arrays, you'd have to instantiate an array that you don't instantiate in this case. 

# Small complaints

`elif`? Really? That's so unsemantic. I'm not even really a fan of `elsif` in Ruby, but this is even worse. In my opinion, Java, JavaScript, etc. win with `else if`. 

True, False, None, etc. are capitalzied. I understand it because they're constants, and it is in keeping with python's desire to keep the global namespace as clean as possible, but it's also different from how pretty much every other language does it and seems unnecessary. 

None. As if the nil/null battle weren't enough of an issue, they had to throw in a 3rd one. 
