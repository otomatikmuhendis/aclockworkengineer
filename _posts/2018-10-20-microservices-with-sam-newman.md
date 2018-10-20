---
author: Olcay Bayram
layout: post
categories: Database
published: false
title: Microservices with Sam Newman
---
Last week, I was lucky to have a workshop from Sam Newman about microservices. I got his Building Microservices book and he signed it with a funny quote for me.

The best thing about the workshop was that it did not start with all that hatred against to monolith systems. If you are building something from strach you can still build it as a monolith system, it is not that bad.

I have worked in a lot of places, real big and real bad ones though and that gave me a lot of experience. When I read books about how to deal with legacy stuff or killing that monolith giant, the worst case scenarios were not even as bad as what I have seen. Still I can solve the stuff with the knowledge in the books but I could not use it exactly as it is. So it is always different in the book and in the real-world. The book cannot help you to solve your specific situation and you help yourself by hiring a consultant at the end.

Sam Newman is one of those consultants, so he knows really ugly stuff. He shared his experience with us in this compact 2-day workshop. We even did an event storming session.

Notes from the workshop;
Log and monitor everything. If you are refactoring, you need to know what you are trading off and is it going to pay off.

Maybe your issue tacking system (Jira) is the best source to see what to refactor. If some related microservices need to be worked on together all the time, then maybe their fate should be considered together. Look at the paper [Hotspot Patterns: The Formal Definition and Automatic Detection of Architecture Smells](https://ieeexplore.ieee.org/abstract/document/7158503)

Change one thing at a time. First split into microservices then change your tech stack if you want to.

"organizations which design systems ... are constrained to produce designs which are copies of the communication structures of these organizations." - [M. Conway](https://en.wikipedia.org/wiki/Conway%27s_law)

[On the Criteria To Be Used in Decomposing Systems into Modules](https://www.win.tue.nl/~wstomv/edu/2ip30/references/criteria_for_modularization.pdf)

[Memories, Guesses, and Apologies](https://blogs.msdn.microsoft.com/pathelland/2007/05/15/memories-guesses-and-apologies/)

[Modelling Reactive Systems with Event Storming and Domain-Driven Design](https://blog.redelastic.com/corporate-arts-crafts-modelling-reactive-systems-with-event-storming-73c6236f5dd7)