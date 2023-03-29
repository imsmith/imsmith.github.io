---
layout: post
title: 'The problem with APIs'
date: '2023-03-22 00:00Z'
categories: post
---
# The problem with APIs

The problem with APIs is that they are dead -- they just sit there like an Easter egg waiting to be discovered, and once found, waiting to be asked a question, and then (maybe) they respond with an answer.  This dead-ness then permeates all the things we try to do with APIs, from building applications to service discovery to security to marketplaces and the so-called API economy.  None of it works in a satisfying way because at the core, is a bunch of dead APIs that stink.  Entire companies exist to try and deal with that stink, and they will all fail because it is the preternatural stink of primordial failure that can never be cleansed from the room.  Anyone who says otherwise is a fool or a grifter. 

What's the alternative?  What is an API that isn't dead?  Is that even the right way to think about it?  

Does everything about an API stink?  The _concept_ of interoperability is awesome, and the Mashup Economy of the early Web 2.0 era is proof of that, but that died out because sharing isn't profitable.   The suits want profits, so the stink isn't because of, or exclusively because of, a technical issue.  

In a technical sense, APIs stink because, as respondents to requestors, they are passive participants in the context they reside in, and because of that, they are like Daniel Radcliff's Swiss Army Man in all his flatulent glory.  The fully realized idea of REST, where you have hypermedia controls and HATEOAS (Hypertext As The Engine Of Application State), and the fully realized idea of the Semantic Web, where you have Linked Data and all the underlying bricks that it is built on like comprehensive ontologies to describe expert systems and rules for making inferences within and across those ontologies, both tantalize with promises that they might be able to solve the stink.  I say "might" because they are both sufficiently complicated that no one has been willing to deploy them in the kind of environments outside of academia, government, or research science that put ideas like these to the cold steel of reality, so they remain mythological beasts in the wilderness of the open market.  Instead, we call JSON-RPC over HTTP "REST" or "GraphQL" because it makes us feel like we haven't failed and we just pretend that the Semantic Web doesn't exist so not to feel inferior to the Holy Orders who have forsaken the pleasures of life to implement it, and in so doing we press on into the darkness becoming accustomed to the stink as it costs more and more every year.  

The alternative is to stop thinking in terms of clients and servers and start thinking in terms of peers.  Peers can have relationships, they can share context, they can be alive.   And unlike clients and servers, they can do all these things without being in the same boundary of authority.  They accomplish this by passing messages that are contextual, semantic, and factual -- in other words, events.  Peers communicate with peers by building a world that is everything that the world demanded by clients and servers is not -- inconsistent, itinerant, intermittent, incomplete, inchoate -- and they do this not with data but with information in the form of facts anchored in time like a play-by-play record of what is happening in a ball game.  

The reason why this shift to events is important is because of what it does to how - and where - you store the data that germinated the information that event contains.  And this, in turn impacts how you design an application, which itself influences which language you might decide to use to encode that design for the computer.  In the olden times when nearly every application on the Web was built on the LAMP stack -- a term for Web applications built using Linux, Apache, MySQL, and PHP -- and that necessarily meant that the characteristics of a Web application were that it stored its data in a SQL data base and that every data record was mutable and that meant that mutability would flow back into the implementation of the application logic even in the best case, and in the worst case it could even flow back into the implementation of the Web server and the operating system which created lots of variables about the environment, the configuration, and the application itself.  Every variable became an opportunity to make a mistake, and every mistake became an opportunity to create a failure, and every failure became an opportunity for an outage.  And because of the nature of threaded systems that share memory, every connection a client made created a new chain of variables, that could lead to mistakes, that could lead to failures, that could lead to outages, which, in effect, meant that the system inherently became more fragile as you added more connections.

And then we put very popular applications on these LAMP stacks and they started to have tens or hundreds of simultaneous users.   This was fantastic if you made load balancers or firewalls.  But it really is an awful outcome if you think about it as an engineering problem; instead of acknowledging that the methodology was flawed, iterations and evolutions of technology that have descended from the LAMP stack didn't change the underlying fact that Web applications become more fragile as you add users.  At best we've pushed the probabilities that an outage will occur down far enough that we accept the risk with many layers of abstraction and indirection, all of which make today's application mostly inscrutable and that makes building, operating, and changing them more expensive.

The fundamental verb of the Web is GET, and that locks it and all the technologies derived from it, into a design paradigm that predisposes one to arrive at a client-server architecture for applications.  In contrast, the fundamental verb on the Internet is LISTEN, and that sends one in a very different direction when deriving a new technology or designing an application.