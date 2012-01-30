---
layout: post
title: "First post"
date: 2012-01-29 16:44
comments: flase
categories:
- HouseKeeping
---

Got [Octopress](http://octopress.org/) up and running. I like the idea
of a staticly deployed blog using a simple wiki-like mark up
language. I lose comments, but part of me thinks that having a blog is
a minimum for participating in the discussion. It's a bit much, but
I'll wait to see if it becomes an issue and figure it out then. For
now, I have a blog, so I'm going to focus on the writing.

Ran into a few issues setting up Octopress - somewhat my own
fault. The first was that I switched from rvm to rbenv. Wasn't really
an issue, but rvm feels more ready for prime time on OSX Lion. I had
trouble getting ruby to build, and had to switch from LLVM to GCC. I
would think that it shouldn't matter, but it seems to. I
[kennethretiz's prebuilt packages](https://github.com/kennethreitz/osx-gcc-installer/downloads)
to be the best install.

After that, got to the point where I do 'bundle install' from the
Octopress clone, and it started complaining about not finding
'stdio.h' when building fsevent_watch. [Others have seen this
before](https://github.com/imathis/octopress/issues/320), but really
the issue is being [addressed](https://github.com/thibaudgg/rb-fsevent/issues/20)
by the fsevent gem. Ends up being some fun differences between LLVM
and GCC again. I switched over to the prebuilt fsevent gem, and that
worked. Just a simple change in 'Gemfile':

    gem 'rubypants'
    -gem 'rb-fsevent'
    +gem 'rb-fsevent', :git => 'git://github.com/ttilley/rb-fsevent.git', :branch => 'pre-compiled-gem-one-off'
    gem 'stringex'

Those are the main build issues, but also ran into a couple of issues
working with Octopress.

The first usage issue is that I'm still trying to figure out the
overall git workflow. You do a clone of the Octopress repo, and then
work within that clone. This seems a bit unclean to me. I want to keep
my actual blog source and the Octopress source separate, even if the
Octopress source is tracking. Add to that that I'm sending the site up
to Github's personal pages for hosting it, so that has its own
repo. Just seems like this will bite me at some point.

The second issue is just getting familiar with markdown. I'm finding
out some of its limits, like how it can't embed block quotes inside
numbered items. Nothing major, just learning myself.

But, Octopress is up and running, and posting to the Intarwebs, so
that has to be considered a success. Now I get to move on to the real
content.

--mac