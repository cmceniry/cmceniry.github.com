---
layout: post
title: "Monitoring Discipline 1: Meter"
date: 2012-10-02 23:52
comments: true
categories: 
- monitoring
- sysadmin
---

In the last post, I talked about 4 different monitoring disciplines. In this, I'm talking about the first one: Meters; and try to distinguish some common Meter patterns.

I pick Meter for the first one for two reasons:

* It is the first one in the pipeline. It tends to be the closest to the feature being monitored.
* It tends to be the mechanism that I (and I believe many others) think about first.

The Meter is the basis of measurement. Whether it's counting the number of bytes going through a network interface, a recording of events generated from snmp traps, a log watcher, or a stream of metrics coming out of an application, the Meter is any item that can be measured or status checked and the path for sending the measurements to consumers who are interested in them. These are two fundamental and distinct sides. In earlier days, they were typically intermingled in one feature or tool, but now technologies allow for different distinctions between those.

There are two main categories of data types for the Meters: numeric, and event based. The primary difference for these is distinguishing between looking at a specific value (or an aggregate or nonvalue items), and looking at a specific event. Numeric values have a continuous meaning - the read at time t will have a different but just as significant meaning than the reading at time t+1. Events on the other hand are not discernable in time - if I receive an event at time t, I may not know the state of the system at t+1 (sometimes I can do another poll, sometimes I can't).

The Meter can use any number of mechanisms:

* SNMP Poll.
* JMX Poll.
* SNMP Trap.
* Reviewing application logs and counting ERROR lines.
* An application that exports mechanisms as a stream to some endpoint.
* An application that sends events to a collector via syslog, a message queue or bus or chat protocol, an HTTP post to a web service, a generic mailbox mechanism.
* An application that sends events via a message queue or bus to a collector.
* A status/transactional check that grabs a web page and checks it for validity.

We tend to want to turn Meters into a push versus pull argument, but it ends up being a false dicotomy. In many cases it depends on the perspective. An application which uses the [metrics](https://github.com/codahale/metrics) will feel as it it's pushing those data points out, but the collection engine that uses it may pull those data points via JMX calls.

The key with Meter is identifying What you want to be watching. Everything else is plumbing (as if saying that makes it all easier).

Note: While it can't be discounted, the storage engine is not necessarily a part of the Meter. In many cases, the storage could also be used by the other disciplines. It's capabilities impact Meters, but it is a technology area more than an area of monitoring work.
