---
layout: post
title: "Language matters: Nitpik with Chef"
date: 2012-02-05 23:14
comments: true
categories:
- Configuration Management
- SysAdmin
---

For a while now, Chef has bothered me. Some of it I can blame on the
server setup (pull/push and exposure of general manifests), but
there's been something that's been bothering that I hadn't been able
to put my finger on... until now.

First, I'm going to come out and admit my bias to a declarative model
over an imperative model. I won't go into why. Burgess does a much
better job [describing this](http://cfengine.com/markburgess/blog_order.html).

My issue with Chef comes down to one word: action.

    package "ntp" do
      action :install
    end

Now, to be fair, this is a very declarative description. If you dig
into what it's doing, you'll find that it's interrogating the system
and only installing the package if it needs to, or alternatively,
it'll make sure that the package exists. If that's the case, it's not
"install"ing every time, it's making sure it's
"install"ed. "Install"ing is a verb (it's saying "action"), but
"Install"ed is a noun (state).

Why does this matter? Let's go back to cfengine:

    commands:
      "yum install -y ntp"
        ifvarclass => "!ntp_installed"

Now, we're further down the slope. This is a command/verb/action -
there's now way around that. We've detached the descriptive goal of
what we want from how we've written. We have to know that our
convention for an installed package is to use that command structure,
and to make sure there's a class that checks for if ntp is installed
or not. The var is in the check, not in the overall description.

So, why is Puppet better?

    package { "ntp":
      ensure => present
    }

There's no confusion around this one. It's clearly saying make sure
this package is present/installed. The magic of how that happens is in
the underlying resource abstraction layer, which is cool in and of
itself, but it's real win is that it allows you to continue thinking
in a declarative way, not in an imperative way. There's no mental
flipping back and forth.

Ok - so why does _that_ matter?

[Language matters](http://en.wikipedia.org/wiki/Linguistic_relativity).
Language influences the way you conceptualize a problem, plain and
simple. The BDD guys hark on this as well. I think for sysadmining
roles to move forward, we need to think in a declarative way. Yes,
there are times for an imperative method, but not in system
descriptions.

So, it's a nitpik, but it's one that can have far reaching impact.

And yes, I know you can do execs in Puppet, so you can shoot yourself
in the foot, but you'll have less misfires with Puppet than with other
tools because of the way it makes you think.