---
layout: post
title: "ANCL: Application Network Communication Language"
date: 2012-03-26 20:16
comments: true
categories:
- SysAdmin
---

I'm starting a new project aimed at handling firewall policies in a
sane way. Well, it's not so much the firewall policies themselves, as
it about how we think about the firewall policies, or more precisely,
about how we thinkg about the application policies themselves.

All firewall configurations that I've been part of have come about in
a specific pattern:

1. Someone writes an application, and puts together some documentation
that, if we're lucky, includes a visio of how the application
talks. Most of the time, it's something closer to a "open these ports
on your firewall to make this happen."
1. A sysadmin/app engineer/gwmth(guy who makes things happen - some
future post) looks over the documentation and identifies specific
hosts (IPs really) based on where the app components are running. He
puts together a ticket asking for specific ports to be open for
specific IPs or ranges. If you're lucky (I'll admit, I've been that
lazy admin), the admin puts together a visio of how it is all laid
out.
1. A netadmin takes the rules from the sys/app admin, tries to match
them up to existing rules or network ranges and adds them to the
firewall. If it's not a stateful device, it's worse than that as
you're limited by the language.

So, we've got a little game of operator going on here. On top of that,
usually the only permanent records of the change show up in two
places: any ticketing system, and the firewall itself. This means that
to try to figure out what happened, you have to go sort through
tickets or reverse engineer it from the firewall itself.

If you've ever had to deal with this, you end up having to reverse
engineer it all the time.

**This doesn't work.**

There's now [tools](http://www.redsealnetworks.com/) that will help
you with this, be more efficient at it. They look at all of the rules
and you can go back and check to see what's approved for what. But the
modelling is still done on a "this IP has access via this port to this
other IP, and this collection of IPs/ports are this model." But it's
still approaching it the wrong way - and if you're more efficient at
doing the wrong thing, you're just doing it wronger.

These tools do other things, but from an application perspective,
they're very late in the game. So, late that it feels like we're doing
network management backwards. I think we gotta go back to the
beginning. Instead of focusing on the network and building rules from
there, I want to start with the application and derive the rules from
there.

And yes, before we go there, documentation is good. I'm not trying to
give people a pass on the documentation. I'm trying to find a way that
makes it easier to document. So, keep that in mind.

Imagine that when you write an application, you can also easily
identify what network communication is needed relative to the
components for you application. For example, look at a simple LAMP
application. It fundamentally, has 3 components:

1. the Database,
1. the Web Server, and
1. the web site consumers.

And we can readily identify what access is needed:

1. The **Web Server** needs access to the **Database** on port
**3306**.
1. **Web site consumer**s need access to the **Web Server** on port
**80**.

It's fundamentally "X needs access to Y on port P" or if you want to
move it around "X is allowed on port P to Y" or "X is allowed P to Y."
If you've done any access control before, you'll notice the same
pattern of Actor(X), Resource(Y), and Permission(P).

I think there's something about this that can be done
[SAML](http://en.wikipedia.org/wiki/Security_Assertion_Markup_Language),
but its focus is on the individual's identity and access
control. Maybe there's something I'm missing, but if I am, [I don't
think I'm in the minority](https://www.google.com/search?q=don't+understand+saml).

So, we have a general form for recording the policy. There's also the
other key take away from this one in that we're not dealing with
IPs. Nowhere above is an IP listed. We're talking about components
(Actors and Resources). This could take the form of:

    {'MyWebSiteModel':
      [
        'WebServer': 'DB:3306/tcp',
        'WebSiteUser': 'WebServer:80/tcp'
      ]
    }

This is just the general description. It's still not very useful. To
do that we have to instantiate it - basically, assign hosts/IPs to
each of the components:

    {'MyWebSiteInstance':
      'Model': 'MyWebSiteModel',
      'Components': {
        'DB': ['2.2.2.2'],
        'WebServer': ['1.1.1.1', 'web2.mydomain.com'],
        'WebSiteUser': ['0.0.0.0']
      }
    }

Based on all of that information, you can form the rules that would be
needed to fit any firewall.

How does this address my initial problem? It comes down to fitting the
abstraction model to the handoff points:

1. Application Developer: Generates the components and communication
patterns.
1. Application Administrator: Instantiates and assigns specific IPs or
hosts to each of the components.
1. Network Administrator: Gets a concise description to work from. If
the right amounnt of automation is applied, I might be marginalizing
the network admin for this task (that's not completely true - there's
plenty of sites that have enough device complexity that no simple
model will be immediately applicable - but it could still be used as a
starting point)..

The model I used above is overtly simple. I already have questions
around RPCs, dynamic ranges, encapsulated protocols (* over http,
anyone?). I'm only starting the discussion here and dealing with the
minimal accomplishable step. It's something to build on.

Now for the practical hand waving. The follow up pieces to this are
the tools to make it useful. We still need:

* A tool for recording this. This could be done via the above forms,
  but it'd be nice to have it wrapped up in a bow. This is doubly
  desired when we have to consider how all of the policies are
  instantiated with the specific host information.
* A tool for visualizing. It'd take in the general descriptions, and
  show a pretty display for it.
* A tool for converting the models to rules. It would take in the
  general descriptions, the specific host information for each
  instantiation, and then outputs some for iptables, pf, acls,
  firewall policies, etc. Yeah, some magic here.

So, first things being first, my [next project](http://github.com/cmceniry/ancl)
is something for tracking.
