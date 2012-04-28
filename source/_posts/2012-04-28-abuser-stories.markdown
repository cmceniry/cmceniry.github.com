---
layout: post
title: "Abuser Stories"
date: 2012-04-28 10:25
comments: true
categories: 
- software
---

Was just listening to [PaulDotCom's security podcast](http://www.pauldotcom.com/)
([283](http://traffic.libsyn.com/pauldotcom/PaulDotCom-283-Part2.mp3))
[NSFW], and they made a comment about "Abuse Cases." I haven't dug
into it yet, but I really like the sound of that. Part of it is a bit
tongue in cheek, but there's more there. I'm constantly subjected to
the Use Case and the User Story, and most of the time, those are
feature focused:

    As a user, I want to be able to store my data...

Now, that's not always the case, as we've been able to get some others
in there:

    As a day-to-day operator, I need to be able to perform
    maintenance on a node.

and

    As a day-to-day operator, I need to be able to collect metric X to
    see the health of the system.

So, there's definitely credence to the idea of "Users stories are for
everyone," or "All of use are users," so why would we need to use a
different term. Couldn't we just say that it's all already covered?

Well, there's two things that I think are ignored there.

First, I'm currently been working a great deal on compliance, so I'm
thinking of it as Role Separation. A User is a very specific role that
is very different from an Operator, or an Attacker, or even subclasses
of those (Store Operator versus System Operator versus Application
Operator).

Second, [language affects the way we think](http://en.wikipedia.org/wiki/Linguistic_relativity).
This is not new, this is not some crazy idea. We have so much baggage
with each term that already exists.

To me, a User Story is feature focused. It explores the Positive
Space. We could include the operational examples as part of
this. These are things we *want it to do*.

On the flip side, an Abuser Story is anti-feature focused. It explores
the Negative Space. It's for things we *don't want it to do*.

Judy Neher has a presentation at Agile 2012 for [Abuser Stories: Thinking Like the Bad Guy to Reduce Security Vulnerabilities](http://submit2012.agilealliance.org/node/13318).
I look forward to seeing what comes out of that.

Yes, I see the slippery slope that I'm on. We could get so prolific
that every type of story actor gets their own top level
classification. The User Stories approach does make you think through
the different roles that will use it, so all of the different top
level labels may be superfluous. There is validity to that
concern.

But with the baggage, I think that adding something with emphasis may
be necessary for a time. Until it's widely accepted about bringing in
the negative space, I'm going to keep using the term Abuser Stories
(assuming I'm not treading to much on victims of violence...).
