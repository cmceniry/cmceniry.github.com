---
layout: post
title: "DNS: Inverting the View"
date: 2013-03-23 12:14
comments: true
categories: 
- SysAdmin
---

This is something that I've thought for a while, but am trying to get
the backlog of thoughts out.

I'll admit that I run bind for DNS because it's a "safe"
default. There's many cons against it, but it does work well enough
for many-many situations. But one of the issues that I've run into is
that how views are managed are counter to how I would like them to be
managed 99% of the time.

Specifically, the Zones are children of Views and when working with
the underlying configuration files, you have to maintain two separate
sets of files (or point to the same files and have the same data). I
want it to be the opposite to some degree. I want to be able to
maintain one zone file which mixes views with some markup on the
records themselves.

This isn't necessarily a bind thing; though, bind certainly sets the
stage for others. It matches most DNS servers because it matches how
the mental model of DNS Zone delegation goes, but that may not be how
the mental model of record management in the context of views should
be. There may also be technical limitation, if you're mixing zone
delegation of children zones (e.g. record bad in zone
child.example.com) in one view and child records that existing
directly . But honestly, in those cases, I think it goes back to the
"that's an antipattern that you really shouldn't be doing even when
you think you should be doing" that gave rise to linting programs, so
I'm going to discount that a bit.

The key here is that when I work on records that are split DNS, I'm
doing it on a per-record basis. I have to ask the question when I'm
looking at the record "where should this be visible from?" In the
context of the per-record, it's much easier to answer. When I look at
an entire zone, it's a PITA to say "where should _all of these_ be
visibile from?"

Another side effect we end up with is that due to this extra overhead
of managing the split zones, we end up with some intenal.example.com
zone. That "internal" starts polluting everything. Yes, it's ugly, but
the problem is more than just aesthetics. If you want to move a host
from one side to the other, or have a host respond to both sides, or
have any SSL certificates anywhere, or not expose that you have a
secret "internal" domain, or avoid any of a whole host of problems
because DNS is so critical to how we run networks, then, well, then
you're sunk.

Again, this isn't necessarily limited to bind, and honestly, how bind
or any other DNS server implements it doesn't really matter. The DNS
server should be responsible for serving out queries. A separate
service should be responsible for the management of DNS. Everyone
wants a nice pretty (for whatever your definition of pretty is)
administrative interface. That is something that is separate and
builds on top of it. Realistically, the admin interface can be very
different from the configuration files. That's part of the whole
point, isn't it? The semantics and languaged used by the admin
interface get mapped into different configuration formats. If you want
to add a new DNS service, all you have to do is figure out the
mapping. The biggest mistake we make sometimes is directly
implementing the configuration format of the specific DNS service back
in the administrative interface. While it'd be nice if they match up,
it's more critical to have the administrative interface match the
model that you believe is the most appropraite for the people using
it, and sometimes that doesn't match the configuration files.

Along those lines, although I like bind from a practical standpoint, I
also like the make of tinydns, largely because it does this well. In
tinydns configuration, you name your views:

        %internal:10
        %external:

Then you can follow up with associating the actual records with each
view:

        +www.example.com:1.2.3.4:::external
        +www.example.com:10.11.12.13:::internal

It's all in one nice neat location to look at.

There's three extensions that I'd make here:

1. The location part should be a tag - so you can have multiple
locations specified in one line. Yes, this is an edge case since most
people only deal with two views (internal and external), but it does
have meaning if you have more than 2 views. For instance, if you have
public, sitea, and siteb, and don't want "bothsites" to be public:

        +www.example.com:1.2.3.4:::external
        +www.example.com:10.11.12.13:::sitea,siteb
        +bothsites.example.com:10.11.12.14:::sitea,siteb

1. The location indicator for tinydns is really an indicator of the
view that you want it to be present in, for whatever properties, of
which clientip/location is only one, apply to the view. Basically, I'm
arguing that tiny-dns should support equivalents to the
match-destinations parameters of bind&#42;.

1. Add some magic for GeoDNS and let it be its own "location"
tag. This is some syntactic sugar that makes it easier to do since
it's so common today and interacts interestingly with views.

&#42; Actually, I'm not arguing that tinsdns should support that, just
like I'm not saying that bind needs to support per-record view
attributes. When it comes down to it, if the proper way for tinydns to
handle the match-destionations parameter is to run multiple tinsdns
instances bound to different destinations, that's perfectly fine. It's
the administrative wrappers or management interface around the DNS
service itself that needs to support this semantic, and then map that
out to the DNS service.
