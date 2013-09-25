---
layout: post
title: "Hack a Search Engine: DuckDuckGo and My First Plugin"
date: 2013-09-25 10:50
comments: true
categories: [Perl,DuckDuckGo]
---

{% img https://s3-us-west-2.amazonaws.com/frncscgmz-blog/ddg.png %}

For about half a year ago I've been experimenting with the search engine 
**DuckDuckGo**, which main selling point is that they don't keep records of your 
searches and value a lot the privacy of their users, unlike Google.
For me this was interesting in the beginning, but what really sold me was 
their Open Source platform, which gives anyone the opportunity to contribute 
by building small functional plugins called **Goodies**.

These Goodies come in various flavors, the most basic ones are the regular 
Goodies, Perl scripts that may take some values, make some calculations and 
return a small snippet of information (basic arithmetic,spelling,
word anagrams, among others).

The other one is called a **Spice plugin**, which works a lot like a regular 
Goodie, takes a couple of values with some trigger values as well, but the 
difference and advantage here is that this kind of plugin is able to 
make calls to external API's, take the response, make a callback to the 
frontend and manipulate it using JavaScript to display it on the search 
results.
Spice plugins make up for some of the most interesting live plugins on DDG, but 
they're also the most difficult ones to develop, so I'll try to show an 
example of one of the regular Goodies I made.

One thing first, developing a Goodie requires some knowledge on the *Perl 
programming language*, but It's not something you can't pick up quickly as 
you go or something really deep. I had no previous experience with Perl, 
save from some quick and dirty one-liners. But the examples and tutorial 
on the official Docs make it very easy for you to learn en come up with 
something fast.

The DDG platform has the advantage that it leverages the full capabilities of 
**CPAN**, you can download and use any supported module on CPAN, all this as 
long as each new module is indicated on the requirements file of the Goodie.

A Goodie file is divided into four logical parts: imports and dependencies, 
plugin info, trigger definition and query handle.

The Goodie starts as any other Perl program, with the definition of the 
program package which must follow the convention:

``` perl
    package DDG::Goodie::YourProgram;
```

Any Goodie should also import the DDG standard package and any other 
dependency:

``` perl
    use DDG::Goodie;
```

The plugin info is nothing more than the meta-data, developer information, 
type of plugin, description, repository url, etc.

Trigger definition is one of the key parts of the program, out plugin should 
respond to a set of predefined keywords. You can defined in what part of the 
search query should be located (*start ,end, startend ,any*).

``` perl
    triggers start => 'keyword'
```

The handle query is the core of our plugin, this is a subroutine which is 
called once one of the triggers defined is activated.
Here you can pretty much do anything you need with your search inputs 
as long as the returned value is a string.

``` perl
    handle remainder => sub {
      return 'String: ' . $_;
      return;
    };
```

Here we use the special variable **$_** which stores the default input to 
the subroutine and we return the complete concatenated string (using . to 
concatenate). 

The lonely **return** function just makes sure that our subroutine returns 
something even if its nothing (I'm aware that makes no sense :-).

After all this, all Perl packages on DDG need to return true when loaded 
correctly. So you need to add the following:

``` perl
1;
```

Yes, I wasted a whole code block on that, but its necessary.

And thats it! Obviously this wont work on it's own.
DDG has its own platform for testing plugins of all kinds, but that goes 
over the scope of this poor article.

DDG's platform has some [excellent documentation](https://github.com/duckduckgo/duckduckgo/blob/master/documentation/goodies_overview.md) 
which covers every detail about the development of plugins and how to deploy them. 
Most of their work is at GitHub, and from personal experience, 
the community is really friendly and helpful. They made my first contribution a breeze.

They also have a community page where people can contribute on non-coding tasks 
by giving ideas on new plugins, feedback and more.

This article may seam preachy, but this project made me very exited because 
it was one of the firsts OSS projects I've ever contributed on and it was 
such a joyful experience that I wish to keep doing it for a long time.

You can checkout the [Goodies repo](https://github.com/duckduckgo/zeroclickinfo-goodies) 
at Github, for some examples.
