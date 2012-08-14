---
layout: post
title: "First pass on clojure-jmxxmppgateway"
date: 2012-08-13 23:42
comments: true
categories: 
- software
---

I've been playing around a bit with clojure. It's nice to cut my teeth on somewhat LISP code. I'm not going to debate a lot of the language merits; I'm just going to say that I'm enjoying it.

I dusted off a project that's been sitting for a bit as it's cropped up again. I'm doing a lot more java work, and am in need to a JMX manager. Well, more that I'm in need of a way of accessing a JMX interface.

Originally, I had been using the [command-line JMX Client](http://crawler.archive.org/cmdline-jmxclient/), but I kept running into 2 problems when running that my monitoring instance (had been using cacti + nagios):

1. The startup time for the jvm for the command-line client was too expensive. When attempting to run it against several different servers and several different metric points, it over ran the polling interval.
1. JMX and firewalls - @wheee.

So, I started by translating JMX to the universal application transport protocol: HTTP. The first project is clojure-jmxhttpgateway. It takes a GET with JMXConn=$hostname:$port, JMXBean, and JMXAttribute as parameters, and passes that back to the JMX engine to get the ways and returns it back. It works pretty well, and took out the gaps in my cacti graphs.

That was two years ago. I made a couple of changes recently.

First, I updated the HTTP gateway to use the latest clojure and ring web framework. The second change was a little crazier. I like XMPP, so I figured I'd see what could come out of it and this is what I got:

    8:33:32 PM Chris McEniry: localhost:4270|java.lang:type=Memory|HeapMemoryUsage
    8:33:32 PM clj@chriss-macbook-air.local: HeapMemoryUsage : {:committed 85000192, :init 0, :max 129957888, :used 8645872}

I like clojure. One of the fun parts - it's only 79 lines of code for the XMPP gateway. Sure, there's not much configurability to it, but that's still a lot to do in such a small set. It's really just a first pass, but it's a fun start.

Some immediate items for the (near?) future:

1. Add security back in - be able to connect to a JMX listener with either SSL enabled and/or user/password auth.
1. Add write access - currently it's only doing reads.
1. Add a second layer of security - since it aggregates several connections, it needs to restrict who can connect to it/what they can request.
1. Refactor the utils from the XMPP and HTTP gateways. Right now, there's a lot of duplicated code which needs to be pulled out. At the same time, I don't want one project that's pulls in too many dependencies - e.g. if you're only running the HTTP gateway, you shouldn't need to get the XMPP libs and vice-versa. In general, the code and just use several cleanups.
1. Need tests.
1. Better config files - i.e. fix the XMPP gateway so that it actually uses a config file.
1. Do the library name spaces "the proper way" - i.e. include domain name in them.

I think I have my work cut out for me.

If you're interested in any of this, check out the github repos:

1. [https://github.com/cmceniry/clojure-jmxhttpgateway](https://github.com/cmceniry/clojure-jmxhttpgateway)
1. [https://github.com/cmceniry/clojure-jmxxmppgateway](https://github.com/cmceniry/clojure-jmxxmppgateway)
