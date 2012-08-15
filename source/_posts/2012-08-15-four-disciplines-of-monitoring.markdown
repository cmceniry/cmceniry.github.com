---
layout: post
title: "Four Disciplines of Monitoring"
date: 2012-08-15 10:39
comments: true
categories: 
- monitoring
- sysadmin
---

(Release early, release often - a lot of the ideas here have some rough edges...)

There's been a lot of run around at work recently about how our monitoring system isn't sufficient. It's something that we all recognize and we want to put effort into fixing it, but I don't think we're all pulling the same direction.

Because we're not.

And to a degree that's ok. There's several different consumers of monitoring (or some variation on it), and each of us has different needs and wants out of it. That's all fine and good.

Where we're failing is trying to formulate our efforts together. Yeah, we can say we all have our own self-interests, or that we're a large company (which we're not really "large") and there's scale issues, or that we're all focused on tools and not as many on the real results (that's certainly true even though not everyone realizes it).

So, I'm trying to figure out the best way to address this. The key is to get us on the same page. Not that we have to have all of our concerns addressed, but more that we have them at least partially identified and shared with others. There's an added issue that we're all speaking different languages, or worse, the same language with many homophones. So, that brings me to this blog post.

There is a lot of confusion between Monitoring and Metrics. And to be fair, there's a lot of overlap depending on how you look at it. In most cases, people see Monitoring for ok/fail status, and Metrics for trending. I feel those are lacking, but I'll be honest in that I can't well articular why.

As an alternative that I can articulate, I see monitoring as being composed of 4 disciplines (areas of study):

1. Meters: The collecting of data points (both numeric and ok/fail).
1. Thresholds: The automatic analysis of data points to signify some event.
1. Responses: The manual and automatic actions that come after Thresholds trip.
1. Presentations: The dashboards, interfaces, and reports that people view, manipulate, and receive.

I picked these pieces because they each have ramifications to various consumers (and producers) of the monitoring system. This do not represent the underlying technologies,but can be used to identify requirements. They also don't represent all of the . I see them as features that users consider important from a monitoring system.

Using these, look at some of the use cases:

* A developer cares about Meters from the standpoints of identifying application Meters that matter, and the API for how to expose or pass those Meters off (something critical to remember is that a poor API will never be used). In many cases, the developer doesn't care about Thresholds or Responses (arguably, if the developer does operational role, yes, but I'm considering that the op role). The developer wants to see a Presentation where they can get information out for their run of their specific pieces, for a load test, etc.

* A planner cares about Meters from the perspective of specifying some application Meters that matter. He doesn't care too much about Thresholds or Responses. He wants to see a Presentation of the long term trends of the running apps.

* An operator cares about Meters from the perspective of having to make sure the plumbing is there to support it (e.g. snmp infrastructure, collectd infrastructure, storage, etc). He needs Thresholds to have valid meaning so that there aren't many false positives. He is typically going to be doing or watching the Responses. He needs to have a Presentation that shows the current ok/fail status of everything, and if he's astute, he'll be watching for unrecognized local trends.

These are high level use cases, and may not reflect directly the role differences that are cropping up. We can take these and be more specific about them, but for this blog, I'm going to be fairly generic.

These disciplines also show some of the differences that have cropped up between devs and ops. Generally (and yes, there are plenty of counter examples), devs care about the Meters and Presentations, while ops care about Thresholds and Responses. This is reflected by the typical tool choices.

* Graphite is a dev tool. Why? Because it focuses on the Presentation.

* Collectd is a dev tool. Why? Because it focuses on the Meter API.

* Nagios ia an ops tool. Why? Because it focuses on the Thresholds.

* I got nothing on Responses as there's some tools out there (knowledge bases, Orchestrator, etc) but nothing that I feel is too common.

Just in here, I have two discussion points that show a bit of knowledge about those who compose the monitoring community. I hope this language will help additional discussions. A lot can be said for each of the disciplines. They probably deserve different posts on their own, but I'm not ready for that yet. Instead, I'll close with a potpurri of miscellaneous comments:

* It's naive to think that each discpline is self contained. They definitely have impact on each other from a practical standpoint.

* Meters have at least two different subdisciplines: indirect Meters and live checks. This should not be confused with collectd pushes versus snmp polls or the like. I put both of those in the first category. The key difference is "Is this an observation of other data (# of bytes on an ethernet interface, # of errors in an error log, average user request transaction latency, etc), or is this an action that I'm performing to get some kind of status (synthetic transaction)?"

* I'm still trying to sort out how single error events (say an error message from a user's transaction, and snmp trap, etc) play with counting the number of errors. The later easily fits in Meter, but I'm not sure how to address the former. In the snmp trap situation, there's typically a "countable" number of traps and sources that those can come from, so one could theoretically create a Meter for each of those. In the user error case, it's getting past "countable." Do you have a separate Meter for _every_ user and does that have to be defined? There is probably some overlap with indirect Meters and live checks, but I'm still thinking about that. (And YES I know I'm useing "countable" incorrectly - I'm trying to signify the scale and it's impact on managability)

* Along the lines of snmp traps (and single error events in general), these can fall into the auto clearing Thresholds, or the manually clearing Thresholds.

* Thresholds can have a combination of static and dynamic (derived from pervious data), current values (e.g. is this drive full?) and projected values (e.g. will this disk fill up in the next 4 hours?), and probably several other dimensions.

* I'm very apposed to a system where you can only determine Thresholds at the point of the Meters. Sometimes the Meter has sufficient information to make that decision (e.g. static Threshold), other times it does not (e.g. Meter dependencies, dynamic Meters, etc).

* Meters and at least Thresholds can also have their own Meters: how often was this Threshold in the tripped state? I.e. SLA counting.

* Meters need to have some variabilty of time in time. If I'm looking at live debugging, I probably want much finer granularity (5 second/1 second?) than if I'm looking at capacity planning. While disk is cheap, can we really record all Meters at 5 second or 1 second intervals all the time? Does that have application impacts? Does it make more sense to say "for the next 15 minutes, record 5 second intervals _for these specific Meters that I'm observing_? (Presentation systems also have to account for this.)

* Responses can be manual (e.g. I see a disk filling up, so I'm going to go clean up temp files) or automatic (e.g. a process sees a disk filling up, so it goes and cleans up time files). Heh - "If you can't make it a process, you can't automate it." The Responses really reflect a maturity of the overall application response cycle. In that cycle, it typically goes: operator sees the alert and responds manually, operator writes up the typical response so other operators can response accordingly, someone comes along and automates the response so the write up can be much shorter. The side effect of this lifecycle is that Response systems include the knowledge bases that the complex/unautomated responses are collected in.

