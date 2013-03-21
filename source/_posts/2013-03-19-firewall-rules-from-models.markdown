---
layout: post
title: "Firewall Rules from Models"
date: 2013-03-19 00:20
comments: true
categories: 
- sysadmin
- network
- abstraction
---

I'm trying to put a little more meat to the bones of my [ANCL
discussion](http://blog.corgalabs.com/blog/2012/03/26/ancl-application-network-communication-language/). Unfortunately,
I can't say that I have a tool for it all, *yet*, but there's a bit
more of the theoretical basis for it. Some of the key requirements
forming around ANCL are:

* It abstracts the components involved - roles are used instead of
  source IPs and destionation IPs.

* It generalizes the communication patterns - models are built using
  the roles; these models can be used in different locations by
  identifying what composes the roles.

* It can compose multiple models together - since the models are
  composed of roles, using the same role in multiple models connects
  them together (caveat in the second side note).

I was reviewing some slidedecks and came across
[Ript](https://github.com/bulletproofnetworks/ript) which has a [good
presentation](https://speakerdeck.com/auxesis/ript-making-linux-firewall-change-management-resilient). It
is a powerful abstraction over iptables. Bonus: It's abstraction can
even be carried to other stateful packet filters, software or
hardware.

Comparing it to ANCL, there's several key differences:

* A partition is not a connection of models, but a combination of
  them. It's an administrative domain which lumps multiple unrelated
  connections together. This is by far the biggest difference and is a
  matter of the mental models.

* The labels aren't the same as roles. They're good for identifying,
  but they aren't then instantiated. There's a single mapping, when
  multiple would be useful. This seems like an implementation detail
  that could be easily addressed.

* It's focus is on specific instantiations of the models. While the
  rules would be portable to different devices, this limits its
  ability to be ported to separate but similar situations. This seems
  like it is a matter of changing conventions (e.g. the naming of the
  labels), but might be more.

The first item is what it really comes down to is that these are two
different mental and description models for the problem. Ript is a
firewall abstraction language, and not necessarily an application
communication description language. That being said, there are a great
many other pieces of Ript, not the least of it being something
concrete, that make it very useful and means I have some work here to
move ANCL to something that can compare.

* * * 

There's also two parts that Ript has also incorporated which I'll be
honest that I don't know how to incorporate into ANCL:

* NATs and SNATs and other translations. In many ways, these are not
  critical to the application communication pattern, at least, not
  critical at an abstract level. It's when it's applied in specific
  contexts where translations come into play.

* "Bad Guy" Rejects. Like transactions, these aren't critical to
  application communication patterns - in fact, these are
  anti-communication patterns. Necessary at times, but not something
  that are accounted for when building the patterns.

* * *

The side note from above is that the roles aren't completely what are
connected to each other. Consider two models:

1. A three tier application model which has a "webserver" role, an
"application" role, and a "DB" role.

1. A DB model which has an "application" role and a "DB" role.

As the application owner of the first model, I would elaborate the
nodes in the "application" role. Most likely, the "DB" would be
specified by the DBA. Conversely, as the DB owner, I'd elaborate the
nodes in the "DB" role, and leave the "application" role to someone
else. In either case, half of the equation is left unfilled. There's
(at least) two ways to approach this:

1. Since the key is to connect the two edges for the
"application"-"DB" together, the real role association happens
there. So, what language should be used to describe these "edge
roles"?

1. The ambiguity can be taken away if we insert "connection points" or
"service" points into each model. In this case, in the application
model, we replace the "DB" role with a "DB" connection point, and in
the DB model, we replace the "application" role with a "DB" connection
point. When joining these models together, we overlay the connection
points.

Not sure which is the right way, so that'll probably comes down to
implementation.