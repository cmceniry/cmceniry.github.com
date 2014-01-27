---
layout: post
title: "More than just Pets and Cattle"
date: 2014-01-11 21:51
comments: true
categories: 
- Cloud
- Configuration Management
- SysAdmin
---

wIt's been said many times many ways that cloud) servers should be
treated like cattle, and not like pets. Looks like the first reference
is
[Bias](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds),
but there are quite a few others:
[here](http://www.computerweekly.com/blogs/cwdn/2013/04/treat-cloud-servers-like-cattle-not-puppies.html)
[here](http://www.gregarnette.com/blog/2012/05/cloud-servers-are-not-our-pets/)
[here](http://www.theregister.co.uk/2013/03/18/servers_pets_or_cattle_cern/)
[here](http://www.dameware.com/cmdprompt/are-your-servers-pets-or-cattle-.aspx)
just the top ones on a google search. The main idea being that we had
this tendency when the servers were fewer yet more longitudinal to
treat them delicately: putting care and feeding into each of them; now
that we (can) have large amounts of short lived instances, we can't be
bothered with the same care.

That's a completely valid way of thinking (it's a great *place* to
be), so I'm curious as to where its limits are. In some ways, looking
at just servers that way is looking at a point in time and
capabilities and thought.

We've all had pet files. Remember that hand crafted config file that
you spent days of your life tweaking to get it just right? Maybe it
was specific to that host. At some point, you groomed it enough that
it became a golden file for your entire environment and you could copy
it and push it out to all of the other servers. Then you pushed it out
using some higher level config management system. Then you moved up
some semantic level and the file itself got abstracted into specific
resources, and those were composited and pushed out. So, files started
as pets, and by realizing that the file was only a model of something
that we actually cared about, they moved to cattle.

Really, pets are pets because you've become attached to them - you
can't clone them, and it hurts to lose them. Cattle is cattle because
it's easy to get another and it's not a big deal if you lose
it. There's a lot of different specific means to achieve these, but
it's these two fundamental classes of properties that enable this
thinking:

1.) It's easy to copy, and
2.) It's easy to handle losing it (enter whatever you want to say
about antifrigilness here).

But thinking about files and servers is so the 2000-noughts. What are
our pets now?

Moving up from the server, is the cluster. Are clusters now the new
pets? or can we treat them as cattle as well? Given sufficiently large
IaaS services and strong configuration management systems and lots of
variable substitution (well, probably more like locally realized
global patterns), it's actually fairly easy to fulfill property #1
above - copying. As for #2, if you have sufficient global load
balancing of any form (DNS, anycast, etc), you can easily route
traffic to working clusters, or more precisely, away from failing
(lost) clusters.

So, pulling further out, our clusters collapse into a service. Is that
our new pet? With even more config and *aaS and some client service
discovery (aka any sufficiently advanced delivery model), you can
certainly copy it. Though, if you lose your source code, it would
definitely take a bit to reproduce the service (get all those coders
together again, etc). What about losing it? Well, if you are a single
feature service inside of a larger service, you might be able to be
disabled, so you can lose it. But what about that larger service? I
think for most businesses, you can't just lose it.

So, that's your pet.

Maybe.

(One could examine businesses and business models and plans and use
the same comparisons, but I think this first point - what makes
something pet versus cattle across various object domains is copying
and dealing with lose - is done well enough, so my second point...).

There's another way to slice (heh) this metaphor: milk. Not all cattle
is used for steak. Some cattle is used to produce a product, bulked up
again, then produce more of the same product. That cycle time might be
a little too short, so the metaphor might make a little more sense by
using different livestock - sheep. Some sheep are raise for mutton,
some sheep are raise for wool (and yes, you can do both, but
still). For the wool sheep, after the wool is reaped each year, you
have to let it grow out again before you can reap it again, all the
while caring for the sheep. The sheep itself stays around, but you
continue to reuse it.

That being said, you can use other sheep for the same purpose because
lots of wool is the same; and sheep have their own way to easily copy
each other well enough.

But you still don't really want to lose a sheep. You still gotta deal
with it going away and getting the replacement there. The same really
applies to larger services (or businesses) - maybe you can copy it,
but you really don't want to deal with it going away.

So, my second point is really that there's a third category between
pets (hard to copy, hard to deal with loss) and (steak) cattle (easy
to copy, easy to deal with loss), and that's of the milk cattle (easy
to copy, but still hard to deal with lose). This last category by its
very nature persists and is modified, rather than being destroyed and
rebuilt each time. All of those things that we had to think about for
when we wanted to change our pets still apply. Maybe it's not to
servers, but the lessons learned are still valuable.

And lastly, not everyone is *there*. And not everyone who is *there*
is *there* for everything that they do (there's probably a mix of
services made of cattle and services made of pets in a lot of
organizations). So don't feel bad. Just figure out which one it should
be and work to improve.

PS Interesting enough, if we do the combinations of the above, there's
the last class: a service which is hard to copy, but you can deal with
failure. I'm not really sure what that looks like, so I'm going to
leave it as an exercise for the reader. I'd be curious if anyone comes
up with something interesting. Contact me.
