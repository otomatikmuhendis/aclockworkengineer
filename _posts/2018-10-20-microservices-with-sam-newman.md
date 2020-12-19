---
author: Olcay Bayram
layout: post
categories: Software
published: true
title: Microservices with Sam Newman
image: /img/MicroservicesWorkshop.jpg
tags:
  - microservices
---
Last week, I was lucky to have a workshop from [Sam Newman](https://samnewman.io/) about microservices. I have his [Building Microservices](https://www.bookdepository.com/Building-Microservices-Sam-Newman/9781491950357) book and he signed it with a funny quote for me.

The best thing about the workshop was that it did not start with all that hatred against to monolith systems. If you are building something from scratch you can still build it as a monolith system, it is not that bad.

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">We replaced our monolith with micro services so that every outage could be more like a murder mystery.</p>&mdash; Honest Status Page (@honest_update) October 7, 2015</blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

I have worked in a lot of places, real big and real bad ones though and that gave me a lot of experience. When I read books about how to deal with legacy stuff or killing that monolith giant, the worst case scenarios were not even as bad as what I have seen. Still I can solve the stuff with the knowledge in the books but I could not use it exactly as it is. So it is always different in the book and in the real-world. The book cannot help you to solve your specific situation and you help yourself by hiring a consultant at the end.

<!--more-->

Sam Newman is one of those consultants, so he knows really ugly stuff. He shared his experience with us in this compact 2-day workshop. We even did an event storming session.

Notes from the workshop;
Log and monitor everything. If you are refactoring, you need to know what you are trading off and is it going to pay off.

Maybe your issue tacking system (Jira) is the best source to see what to refactor. If some related microservices need to be worked on together all the time, then maybe their fate should be considered together. Look at the paper [Hotspot Patterns: The Formal Definition and Automatic Detection of Architecture Smells](https://ieeexplore.ieee.org/abstract/document/7158503)

Change one thing at a time. First split into microservices then change your tech stack if you want to.

"organizations which design systems ... are constrained to produce designs which are copies of the communication structures of these organizations." - [M. Conway](https://en.wikipedia.org/wiki/Conway%27s_law)

[On the Criteria To Be Used in Decomposing Systems into Modules](https://www.win.tue.nl/~wstomv/edu/2ip30/references/criteria_for_modularization.pdf)

[Memories, Guesses, and Apologies](https://channel9.msdn.com/shows/ARCast.TV/ARCastTV-Pat-Helland-on-Memories-Guesses-and-Apologies/)

[Modelling Reactive Systems with Event Storming and Domain-Driven Design](https://blog.redelastic.com/corporate-arts-crafts-modelling-reactive-systems-with-event-storming-73c6236f5dd7)
