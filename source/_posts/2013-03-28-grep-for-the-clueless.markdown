---
layout: post
title: "Grep for the clueless"
date: 2013-03-28 23:10
comments: true
categories: [Unix,Shell]
---

The first time I had the pleasure of working in a \*nix-like environment with no 
graphical interface was a daunting task. All the comfort of a pointer and 
the cute little icons was gone, all you have before you is a cold and menacing
black windows with a blinking cursor.

Once the initial shock was gone and I needed to actually get shit done
was when I started to learn properly how to move around and use all the tools
available.

I was hooked.

Daily use and constant research made me feel comfortable, but if there was one
thing that to this day is still a fascinating is the grep program and how
it integrates seamlessly to the output of everything else. Later I learned that
this was more of a Unix thing.

So I want to share some of the little things that helped on my constant and
never-ending path to suck less at \*nix.

### The Super Basics ###

The basic syntax used on the grep program is

grep [options] regexp [files]

Where [options] represents all the flags which you might want to include
to manipulate the results of the program.

regexp is the string of characters or regular expression the program needs to 
match.

And finally, [files] represent well...the files where you want to look for that 
particular regexp.

If I want to look for a word on a specific file, I would have to use grep in
its most basic form.

``
grep conf Rakefile
``

This will print every line that matches the given pattern ("conf") inside Rakefile.

You need to look for more than one match? No problem, just append a pipe (|) character
between each match in the regexp argument.

``
grep 'warning\|error\|critical' /var/log/messages
``

### Flags ###

There are tons of flags you can append to the grep command get the exact output we need.
Most of them manipulate the results gathered from the search pattern, others
define conditions to the pattern searching (ignore some word, search recursively, etc.).

Just to name a few.

-c : Instead of listing every match, just count the results and print it.

``
grep -c 'import' AbstractSingletonProxyFactoryBean.java
``

-l : If grep is applied to a certain directory or a set of files this flag
only prints in which files the pattern matched.

``
grep -l '^#include' /usr/include/*
``

-r : So you have a bunch of files where you want to search, but you don't want
to apply grep to each one. You can recursively search in a directory for a 
pattern in all their directories.

``
grep -r 'ERROR' /usr/local/tomcat/log/
``

-v : If you need to dig into a file where theres a pattern that repeats 
in every line but you need to find where this pattern doesn't appear.

``
grep -v 'conf' Rakefile
``

-w : Grep by default always tries to match the pattern even if its between other
words ("conf" matches in "config","configs","ifconf").
If you need to force grep to just select the lines where the pattern matches
the word do:

``
grep -w "conf" Rakefile
``

### Piping ###

Like any good Unix program, grep supports pipping between different programs that
share a common text output.
Say, you're tailing in real time a log, but you want to print only the lines that
contain a certain tag.

``
tail -f MyLog.log | grep DEBUG
``

### A Little Tweaking ###

Some more color in your terminal its always a good thing, it helps highlight 
the things that matter in a sea of white garbled text.
You could pass the flag --color (--colour for our British friends) every time you
use it, but thats annoying. Instead, add this to your shell alias configuration 
(bash_aliases if you're using Bash).

``
alias grep='grep --color=auto'
``

This adds out color flag in an alias every time we use it.

This isn't intended to be a full fletched guide on grep, but if you need more
knowledge on the subject you can always check the [man page](http://unixhelp.ed.ac.uk/CGI/man-cgi?grep) on grep.
