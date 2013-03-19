---
layout: post
title: "A tale of two PaaSes"
date: 2013-03-13 20:38
comments: true
categories: 
- SysAdmin
- Cloud
---

I spend a good amount of time trying to figure out if my operational
team can do much to make the general engineering efforts more
productive. We've followed the usual turns around self-service IaaS
and the like, and we're now exploring the next level of
Platforms-as-a-Service. In exploring the options, I'm seeing two large
patterns.

On one hand, there are the "middleware centric and injection based"
PaaS models. These are the ones where the developer picks a
development middleware (Java Servlet, PHP, Node, Rails, etc), and adds
other parts in. As if by an after thought, a static file service is
added, or maybe a data persistance (i.e. database) service is
added. On a implementation level, these usually involve allocating
some compute and storage resource (e.g. a VM), installing the
middleware container, doing a baseline install of the add-ons, and
starting them all up inside that VM. There are some other
configuration items such as pointing it at some version control
repository, but also the developer is able to login onto the VM via
shell.

On another hand, there's a "service focused" PaaS model. This feels
like the lesser named PaaS, though it probably has a larger install
because this is the model that AWS largely is. In this case, the
developer picks different service components (e.g. DB, cache,
messaging bus, etc) and composites them a bit more
independently. Underneath the control layer, each of the component
providers can implement their services in different ways - using
different VMs, processes, or internal containers (e.g. DB schemas w/
authnz) - based on what makes sense for that provider. There's more
work for the developer here as they have to compose services across
different providers, and the developer doesn't have direct access to
the underlying system, but in exchange, might have better options.

From an implementor's perspective, I think the service focused model
is easier to maintain. This may not necessarily be the right reason to
go down that route, but when it comes to delivery, that matters a
lot. It's also a bit more transportable - at this point in the
industry's lifecycle, it'd be easier to migrate from one IaaS or
traditional Infrastructure to another. It's also easier to extend this
model to other (traditional?) services such as monitoring. You can see
this in the industry - there's many different service providers
focusing on a narrow niche offering around one specific service, but
fewer middleware centric vendors and even those that exist tend to
also include some service based model for the add-ons.

As I said, most of the traditional services called PaaS are the
form. So, what makes the application middleware so much different than
a data store? or a caching layer? Fundamentally, you have some level
of "service" which you want to present a clean interface to. This is
true for the database as well as for a java servlet container, yet
somehow we treat them a little differently in our heads. The only
reason I can imagine is that is where time is spent. As a developer, I
spend most of my time in the code, so that's where my mind goes. But
while I run it, I want to have a better idea of how it fits in with
the other component services.

I think the vote is still out on which way has better long term
viability. And it may never be decided on. It may just be a matter of
preference.

Maybe PaaS isn't the right term for this second model. These are more
Services-as-a-Service, which seems likely to be a great way to confuse
people. Mmaybe they're more along the lines of Infrastructure, and are
just a different take on that. I'll admit, I'm not sure what the right
way to refer to them is, but I believe that the use case that they
present is more than just an implementor's fancy. It's a valid use
case based on how developers are expecting to work with it.