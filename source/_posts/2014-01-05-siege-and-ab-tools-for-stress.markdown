---
layout: post
title: "Siege and AB: Tools for Stress"
date: 2014-01-05 21:01
comments: true
categories: [Linux,Tools]
---

Most of the time unit testing and integral test just aren't enough, specially 
when you're working on exposed applications where you have no exact idea how 
much they're gonna be used or if they have to do long and heavy processing 
concurrently.
I learned that the hard way, and I'm pretty sure I'm not the only one who 
went through a similar ordeal.

Enter the **Stress Test**.

Stress testing, as the name implies, consists in thoughtfully and deliberately 
testing the stability of a system often to the breaking point of it.
This task may seam like a drag to most programmers, specially if they're working 
with a QA team.
Theres no shortage of articles and opinions saying how testing is the 
responsibility of the developers too, and I want to prove that this kind of 
tests are not necessarily a hindrance or a waste of time, they can be pretty fun!

I've used two really handy tools for my stress tests on HTTP servers (REST 
services more specifically).

### Siege ###

The first one is [**Siege**](http://www.joedog.org/siege-home/), its a fantastic open source benchmarking and 
load-testing tool for any of the \*Nix flavors.
Siege is packaged in Debian based distributions so you can just installed 
directly from the repositories, if not you can always get the latest version 
from the official [Siege repository](http://www.joedog.org/pub/siege/).

Once installed, Siege is incredibly easy to use. If you're familiar with 
"Unixy" applications you're gonna love Siege.

If you want to simulate 30 simultaneous users hitting a single service/page 
with a delay of 5 seconds between each hit for 2 minutes this should do the trick:

``
siege -d5 -c30 -t120S http://website.com/hello.php
``

The **-c** flag tells Siege how many simulated users will be executing the 
service concurrently. **-d** indicates the delay in seconds between each 
"user" hitting the service. **-t** tells Siege for how much time will the test 
run until it stops (if this flag is not provided Siege will continue to run 
until its interrupted manual, Ctrl-C most of the time). And finally the URL of 
the service/website.

Its also possible to run a suite of different services in a single test 
instead of running them individually.
You can specify a list of URL's in a text file and use it for your tests, Siege
will execute each service one by one or at random.

``
siege -d5 -c30 -t120S -i -f sitetest.txt
``

Most of these commands should be familiar except the last 2, **-i** generates 
user simulation by randomly hitting the URL's in the text file which is specified 
using the **-f** flag.

While siege is running, each hit prints simple yet detailed information about
the request sent/received. Protocol used, response, delay of response, 
response size and finally the service that its consuming.
When the Siege test its over, a small recap is displayed of the test results, 
for me this is one of the best things about Siege, it gives you straight data 
which is easy to understand.

{% img https://s3-us-west-2.amazonaws.com/frncscgmz-blog/stresspic2.jpg Finished Siege test %}

### ab - Apache Bench ###

[ab](https://httpd.apache.org/docs/current/programs/ab.html) is another fantastic benchmarking tool made by the good folks at Apache. 
As Siege, this tool also comes packaged in most Debian based distos and it's a 
requirement for most of Apache's server tools.

ab is also easy to jump on but it has a lot more depth than Siege on the 
things yo can do with it. Here's the previous test we ran on Siege but now 
using ab with some slight differences:

``
ab -n200 -c 30 http://website.com/hello.php
``

For consistency's sake, the **-c** flag indicates the number on concurrent 
"users" hitting your service. **-n** sets up the number of requests the test must 
complete before finishing the test.
You can also use a time limit instead of request limit using the **-t** flag, 
it works just like the -t flag in Siege except ab takes time in milliseconds.

One thing I was able to do in ab that I couldn't in Siege was testing POST 
services which required a JSON file in its request.

``
ab -n200 -c 30 -p {"jsonfile":{"value":"the-value"}} -T "application/json; charset=utf-8" http://website.com/hello.php > testresults.log &
``

There's a little more wizardry going on in this one. **-p** indicates a 
file(JSON file in this case or a regular file when the path is present) 
containing data to POST. **-T** sets the Content-Type header for POST/PUT data, 
if you're using the -p flag for posting it's necessary to setup the Content-Type header.

Unlike Siege, ab prints the test results only when everything has finished, 
thats why we are using a ">" option to print all out results to an external file.
And finally the "&" character it's a pretty standard option in Unix environments, 
this indicated that this task should run in the background and once run 
it immediately gives back command of the prompt to the user.

ab's results are much more detailed than Siege's results, personally I find some 
of ab's results not that necessary, but it's always good to have the flexibility 
to have all this data on hand.

{% img https://s3-us-west-2.amazonaws.com/frncscgmz-blog/stresspic3.jpg Finished Apache Bench test %}

Both ab and Siege are really handy tools with tons of functionality and a lot 
of neat little things that are beyond the scope of this lowly blog post(I don't 
want to be that guy but seriously read the manuals, everything you might need 
its there).
I really can't decide to preach for just one tool, where one falls short the other 
one can excel, they complement each other well and should be enjoyed equally.

**One word of warning:** *If you're like me, once I got to know these tools, I started 
to get naughty ideas about its uses. Please refrain from using them against 
servers you're not responsible, it's considered impolite if not straight up illegal.*

