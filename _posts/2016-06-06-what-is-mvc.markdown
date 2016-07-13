---
title: What is MVC?
author: phantomdata
date: 2016/06/06
summary: A deep dive into the stylings of an MVC architecture
layout: post
---

*Note to the reader:* MVC is a general concept which lacks a canonical definition.  As such, it is the subject of great debate and several different interpretations.  This piece is based on my subjective interpretations and opinions on the matter.  Personally, I come from a background in Rails and .NET based technologies and as such my opinions are colored to match that of those two technologies' communities.  Your definitions and beliefs may vary, and I appreciate that.  It is what allows the software development industry to continue moving forward and me to never have to write Perl again in my life.

# Introduction

Most web application developers today are fairly familiar with the term "MVC".  As a community, we understand roughly what it means, generically where things need to go and are somewhat familiar with its imposed organization structure.  This design pattern is relied upon by most modern back-end (and even some front-end) frameworks and has become a cornerstone in the daily lives of the majority of software developers.

As with any tool in a developer's belt, it is important to fully understand the nature of the tools with which you work.  In this case, the "fairly familiar" knowledge of the MVC design pattern lends itself quite adequately to basic applications with a 1:1 database mapping between objects and their corollary views (generally, CRUD).  In order to continue growing as a craftsman and further your ability to build useful and maintainable code; a firm understanding of the underlying principles which drive this foundational component of modern software development is in order.

To that end, this paper serves as an exposition and explanation of the MVC design pattern.  It describes the concepts surrounding this design pattern, its constituents (the models, views and controllers) as well as additional modifications and support that can be provided by extension.

# An Overview of MVC

## Generally

MVC is an acronym derived from this pattern's primary components: Model, Views and Controllers.  During a typical request to a web application, leveraging the MVC pattern, each component is leveraged in turn as a request's orders are fulfilled.  These components, can be viewed and in fact are often referred to as layers.

Multiple trips may be made between components of the individual layers; but the typical request moves from the upper layers down to the lower and data is returned back up again.  It is extremely atypical, perhaps even *against* the pattern, to have data requested from a lower layer to the above or have a higher layer reach directly way down below.

To put more plainly the way a request moves through a typical MVC style architecture, please consider the following sequence:

1. A text document is requested from the server.
1. Magic happens.
1. Request hits the Controller.
1. Controller interfaces with a Model Layer to request Models - in Rails this will typically be in the form of ActiveModel talking to ActiveRecord, in .NET a bonafide DAL or Repository, in PHP perhaps Doctrine.
1. Model Layer returns Models up to Controller.
1. Controller then hands the Models up to the View.
1. View is composed, rendered and sent to the client.

That is MVC in a nutshell.

If you prefer analogies, imagine the server at your favorite restaurant.  They receive your order and promptly translate it from "I guess... um... I would like a hamburger...  but, maybe no onions on it?  Can you add ketchup too?  Suzy!  Stop playing with that!  Oh...  a side of french fries would be good...  oh!  Can you add a slice of avocado to that burger?" to "1 burger, no onion, add ketchup, add avacado; 1 side fries".  Your server, in this analogy, is the Controller fielding a request.  

They then hand this order back to the cook (typically your data layer or ORM) who takes their raw materials and constructs a Burger (model) and FrenchFry (model) which is promptly given back to the server.  Your server then places all of the food artfully on your table with a flourish, announces the contents and returns to their merry way.  This was the assembly and delivery of the View.

Incidentally, for many web developers today, MVC represents the first Design Pattern that they have unknowingly grown to learn and love.  It can certainly be argued that it is an architectural pattern, or a standard layout or perhaps a template - but at the end of the day, it is a repeatable pattern to solve one of our most common problems that is easily communicated to developers across many different languages and frameworks.  Design Pattern, who knew?

## A Brief History

MVC represents what was a fundamental shift in the way in which modern web applications were built.  Prior to its gained popularity, quite narrow-mindedly attributed to Rails in this author's opinion, the world of web application development was a mess of competing styles and mish-mashes of various architectural styles.  You might have found a PHP developer writhing through a spaghetti mess of SQL strings in a 10,000 line script toiling alongside a WebForms developer methodically adding a single property for propagation through 14 layers of enterprise infrastructure.  At the same time, developers working on other code bases were developing the same things in completely separate ways.

During the introduction, I described MVC as a *design pattern*.  When discussing design patterns, most developers wax poetic on the nostalgia surrounding the Gang of Four's seminal book on design patterns.  Complex Java constructs are conjured, factories appear along side observers anxiously awaiting the chance to deliver a message up the chain to other implemented patterns.

In the end, a design pattern isn't simply a prescriptive instruction manual on solving a particular problem.  It is a *repeatable* and easily *communicated* pattern for solving the same problem that we keep banging our heads against time and time again.  With that in mind, I submit that MVC is in fact one of the most used design patterns of modern web  application development.

Returning now to the myriads of developers (myself having been among them), all toiling away at their personal empires of code and style; MVC began slowly creeping its way into the common developers' mindset.  It provided a fundamental shift from the previous vastly differing methods towards something that not only modeled the state-less web that we all live in, but gave us all a common ground for solving our business' needs and widening our bandwidth.

## Interpretations

It is important to note; the MVC's pattern's sort-of canonical record is within the Gang of Four's seminal "Design Patterns" book.  Even then, Design Patterns is a bit long in the tooth and presents it as a desktop oriented design and does not hint to its revolutionary nature or explain it in the same way that we do today.

Since there is no real canonical record of what we use today, and the Internet loves arguments, you will find exactly 325,453,120.2 arguments describing differing and vociferously defended interpretations of what MVC means.  The acronym's meaning, the separation of concerns and general concepts remain the same - but definitions surrounding what exactly a Model, View or Controller is and how they interact may be different.

You will even find competing design patterns, that largely lost out when the MVC pattern hit the scene - see MVVM for an example.  It is a slightly different concept, which places a ViewModel in place of a Controller and modifies the way in which presentation is handled.

This text is intent on describing my *general* understanding with the concept of MVC.  It is not for desire to prove correctness, or to further its reach - but instead to pick a single implementation that may be used as a basis for discussion.  Different ideas are the foundation upon which we continue to move forward as a development community.  Never forget that.

## Benefits

So why did MVC take over the development world?  How could a single pattern erode and wash away years of entrenched WebForms developers; how could such a fundamental way in which we architect web applications manage to sweep aside John's intricately designed PHP architecture leveraging a custom view handler, unique and efficient data caching and secret chants?  The answer stares us in the face every day we work on a new project; it was simpler for everyone.

Wait a minute; I hear you asking.  Writing PHP scripts with SQL in a header include was simpler than several layers of abstraction.  How is this *simpler*?

You see, while John the Architect had spent many years building and tending to his beautiful creation, Jane was just starting on the team.  She struggled to understand how to add even the simplest of requests to the system and was baffled at every turn.

One of the ways that MVC makes our lives as developers simpler is that it gives us a common architecture.  It is one further step in the common goal of software development, to just let us solve the business problems and move on.  MVC, within a given framework, is understood by all of its contributory developers.  One project is going to be roughly the same as the next which is roughly the same as the one after.  

You need to add a property?  Go add it to a Model, maybe poke it into the Controller and then make sure to display it in the View.  This pattern will be roughly the same for each project that you encounter and obviates the need to spend overly long understanding how to actually understand the things you need to understand before you can understand how to solve the business problem.

Furthermore, simplicity of software is not simply in the quickness one may operate - but more refined, it is the quickness one may operate reliably.  MVC provides for us an architecturally sound system in which we may express our business problems, with a reasonable amount of safe assumptions as to their long term sustainability (from a software perspective).

Quite simply, it is a proven and reliable means to constructing long lived web applications.  It promotes:

1. A well-thought out and tested approach to separating concerns
1. Logical placements of code
1. Ultimately building maintainable code

After all, if code could simply be written, tested, validated and forgotten about - would we truly care to be craftsman?

For those who may not fully understand Separation of Concerns; it is the concept that each component in a computer system should be discrete and self sufficient.  It bespeaks the idea that if you are attempting to understand a given piece of the system; from the developer's perspective; you should be able to consider one and only one location for its definition.

Without an enactment of Separation of Concerns, you achieve the lauded status of having written Spaghetti Code.  So named due to its graphical (that is, when method calls are graphed and dependencies displayed in an image) appearance to a plate of spaghetti.  Which would you prefer, to find the middle of strand 251 from a scalding hot plate of spaghetti or from within a neatly categorized and numbered system of drawers which range from 1 to 255.  I wonder which drawer it could be in...

MVC posits that the primary concerns of an average web application are that of tending an incoming request, retrieving data for that request which has a specific representation, transforming that data and ultimately displaying it back to the requestor.  These primary *concerns* are what MVC helps us, as web developers, *separate*.

You will, of course, find further need within a reasonably sized application to further separate your concerns; perhaps into modules, perhaps into services or perhaps in Rails...  concerns.  That is a topic that is as nuanced as it is deep and quite rife with Internet arguments.  Enjoy your exploration.

With these concerns separated and typically defined, we are free to *assume* and be reasonably sure we are correct as to where new code should go or old code can be found.  This allows us speed and reliability when working within an existing system, assists in team environments and even helps us the next morning when we return wondering what in the hell we were thinking last night.

At the end of the day, I believe that MVC's greatest contribution to our tool-belt is the ability to easily write maintainable code by both novices and accomplished professionals alike.  The simplicity of its understanding, which allows forgoing explanation, along with its insistence on following its logical standards affords us one of the great treasures of the software development community.

# MVC in Detail

## Introduction

In the following sections I will define, more specifically, the components which make up a typical MVC application.  Again, this is primarily from the Rails perspective - but the general concepts apply to the other views as well.  You will note that I have chosen to place these in the order they follow in a request, to make it more digestible for the reader.

## Controllers

A Controller's responsibility within the system is to handle coordinating the actions of layers beneath it.  It is typically the component with the widest sphere of influence and understanding of the system in its entirety.  They typically fulfill the following primary needs within the system:

1. Handling incoming requests
1. Processing and transforming parameters
1. Communicating with a Model layer to retrieve data
1. Transforming that data and presenting it to the View

The handling of incoming requests entails that the Controller is the first responder within our written code.  Though technically it is not the first responder in our controlled system, as that honor goes to the networking stack on our server; this is the first chance that we as developers typically get to recognize - "Aha!  A user *wants something*!".

The Controller is actually instantiated and its relevant action called by an outside entity.  Most times, this will fall to a routing engine.  This, however, is outside the scope of this document.  Suffice it to say, your controller actually already knows what that *something* is by the time your code is running.

In a Rails application, as is true also for .NET MVC (hereafter, simply .NET) - the Controller *typically* maps to a particular resource (or object or model) within the system.  If your system is built to facilitate the purchase of Books, you can bet that there's a BooksController somewhere in your bag of tricks.

Once that request has been recognized and the appropriate action called; a Controller will typically begin parsing any parameters which were passed to it.  These may, as in our Books example, include an author to filter by in the form of `?author=Frank%20Herbert` or a pagination paging indicator in the form of `?page=2`.  At this point, the controller will read those parameters and typically store them for further use during the request handling.

Since the Controller's action is predetermined (let's say, *index* was called) and the parameters are now in hand - a Controller will typically begin making calls to a Model layer.  This may differ based on your language of choice; it may call to a Rails ActiveModel class that yields ActiveRecord scopes or a .NET Service layer that may return items from a Repository.  Irrespective, the typical pattern dictates that the Controller calls to some form of a Business layer responsible for the ultimate retrieval or modification of Models.

If this were a modifying request, say `POST /books`, then the Controller will likely simply yield a success object up to the View layer and consider its job finished.  If, however, this were a retrieval request, a Controller will typically seek to transform the data returned by the Model (or Business) layer before yielding it to the view.  This may take the form of collecting the objects into an array and freezing their values, paginating a list of objects or perhaps composing a more complex ViewModel in the process.

At the heart of it, the Controller's final throes end with it yielding its gathered and transformed data up to a View layer who's job is to render (typically) everybody's favorite HTML.  From that point onward, this component's needs are largely fulfilled and it may go quietly into the night.

### Common Pitfalls

Controllers are sometimes one of the most difficult portions of an application to control.  It represents the first entry into the system, and thus the simplest place to immediately place items - much like that overflowing table right next to your door.  I know you've got one, we all do.

With that in mind, its important to actively persevere against the tendency to grow a Controller's method to place beyond its reckoning.  Remember, a controller's job is that of a translator and an intermediary - it realistically isn't meant to perform business logic or data persistence directly.  A well crafted application will allow those needs to be performed elsewhere through a simpler and easier to understand API.

This becomes especially true when you consider that many web applications are quite concerned with themselves.  Sure, the business case is that you need to sell Books - but you also need to track how many times the user has visited your site and offer them that free book on their 15th visit.  It is all too easy for this code to end up in a Controller instead of in a separate module where it ought to be.  Consider the usage of a Concern or separate Service class to facilitate this arrangement.

Remember, a Controller *controls* things - it doesn't actually *do* them.

## Model

The model layer is, perhaps, the more confusingly named of components within this design pattern.  It represents what we classically called the Business layer; which means that it handles the business.  Its chief purpose is:

1. Representing
1. Understanding
1. Managing
1. and Enforcing Constraints

within the business problem domain.  If you are looking to understand why your Book can't be sold today, this is where you'll look.

### Models

The reason its nomenclature is found confusing, is that it directly maps to *one* of the items under its control - Models.  Models represent individual units of a given problem domain.  You may find Model classes that represent Books, Users, Animals, Cakes, Employees or Documents.  Depending on the framework you are operating under, a Model may be Fat (it contains lots of logic) or it may be Skinny (it contains little logic) or it may even be POCO (it literally only has attributes).

These Models will typically contain (in one form or another) definitions as to their attributes; BindingType, Username, FurColor, IcingColor, CloseRate or perhaps MarginSize.  They will also typically include validation qualities which dictate what constitutes a *proper* object of that type.  Taken by themselves, they can simply honor the Representation of a given unit within the problem domain.

Where frameworks diverge, is how much this model should be capable of.  Some definitions (.NET) insist that they be capable of little to no actual interaction with the remainder of the system.  They eschew this towards Service classes (which, confusingly enough are also included under the Model layer) to handle the remaining qualities of the Model layer.

Rails, on the other hand, falls into more nuanced camps.  Some argue that a Model should contain the preponderance of logic associated with (all of the code related to how a book should behave), others argue it should contain none of the logic and some fall in between.

I personally fall in the in between.  A Model represents the first place you will visit to determine what an object is about.  I believe it should contain a small amount of information not only on what it is, but also how it behaves.  As your model grows, and becomes *fat*, you can begin grouping and slicing off chunks of that model into Concerns or into Service classes.  I tend towards Concerns for logic associated with an individual item and Service classes when considering aggregates.

### Business Logic

I spoke previously about a *problem domain* and feel I should expand on that.  Throughout your life as a software developer, you will frequently encounter a method of development entitled Domain Driven Design.  The particulars of this are un-necessary in explaining MVC, but the general concept is to model your application after its problem domain.  In essence, move away from contrivances and abstracts and represent your data intuitively to its design.  If its a Cake, call it a Cake.  Create solutions to managing your Cakes around that Cake.  It puts forth that you should modularize, not necessarily based on technical segregations (code that manipulates aggregates) but instead on domain segregations (code that decides icing colors for cakes).

What this entails is the definition and *understanding* of what's termed business logic.  As a developer, you don't particularly care if an Icing Color is determined by the availability of 2 tubes of Royal Blue icing to be mixed with 1 tube of Scintillating Yellow or that it needs to be a working day and 2 days notice is required except before Christmas because there's loads of staff.  Your life would be so much easier if requirements could be expressed in code; but they're not.

These constraints, that I just described, are not imposed by the developer themselves (GitHub leads me to believe all programs would naturally tend towards becoming a JavaScript framework if we were left to our own devices) but instead imposed by...  the *business* for which we work.  Thusly, this logic becomes known as *business logic*.  It is all of the *stuff* that goes into what makes software actually do profitable things.

In MVC, this business logic still falls within the bounds of the Model layer's purview.  Exactly how it is represented, is a deep design decision requiring either a keen eye or years of experience.  Whether you choose to implement logic in a model, a concern, a service class or perhaps even a separate service application itself is a nuanced and fine decision that usually depends on the complexity, relatedness and re-usability of a given piece of code.

Since this text is meant as an explanation of MVC, I will leave it to the reader to explore and understand when to break apart code.  Steve McConnell, Martin Fowler and Avdi Grimm all have excellent books on the subject which I suggest you read.

### Data Persistence Interactions

In the overview sections, I mentioned that each of the components of an MVC application represent a downward (and subsequently upward) flow of information.  Not sideways, not skipping layers, but turtles all the way down.  Rather than writing an "INSERT INTO Books (BookName) VALUES ('What is MVC')" directly into a Controller (or the horror when we used to just shove that into a .php script) we allow a Data Layer to enact this.  This Data Layer is spoken to by the Model layer, thus abstracting and separating its interaction quite well away from that Controller at the start who wants to do everything.

This is a bit of a contentious point, not all MVC implementations strictly include this in the literal Model layer of an application - but may instead move for a Service layer above it to interact.  All the same, it falls into a realm most often lumped into *business logic* and not *controller logic*; which means - in the Model layer it goes.

Please note, this is an *interaction* with a persistence layer - not the actual persistence layer itself.  In Rails, you may call my_book.save or in .NET you may call BookService.save(myBook) - in both instances, you are calling a save method on a model that appears to save the object.  In a well architected MVC application, however, its not.  That save method is actually going to run a gambit of other *business logic* (validations, pre-save hooks, dependent object handling, etc) before handing it off to the persistence layer.

In Rails, that ActiveModel object is actually calling out to ActiveRecord (our ORM) which actually handles making any necessary database calls to persist it.  Likewise, the BookService, which I mentioned before, is actually (likely) calling to a data Repository somewhere that is also abstracting away the actual database interactions.

This is a prime example for a lesson in MVC.  It is not a law.  It is simply another design pattern which has its uses.  Every problem will not be solved by MVC, and data persistence is one of those.  It is generally forgotten about in the case of Rails and not considered in the architectural definition of MVC in .NET, but the actual act of opening a socket to a database server, managing that connection, and streaming sql queries to it are not typically part of an MVC application.

The lesson presented here is that occasionally, you will need to step outside of the MVC concept to cleanly accomplish your actual goals.  MVC is great at representing handling a request to handle web-based information, but falls apart when you apply it too liberally to other areas of your application's needs.

## Views

Views are perhaps the simplest layer to discuss.  In actuality, they are not necessarily code at all - but typically are actually a *template* into which data from the Controller is injected.  Yes, they may contain some *display* logic - but at their heart, they are a template.

A view may take the form of something that resembles HTML, JSON or XML.  Of course, it may also be CSV or TOML or any number of other formats; but it typically looks a lot like the thing that gets rendered out to the client.  You may find that they have fancy names like ERB, Razor or Haml - but at the end of the day, they just (more-or-less) translate to text.

An item of note, is that if you find yourself writing large swathes of logic into views - you should consider modifying the data which is being passed up by the Controller.  You should rarely find complex code and never require class or method definitions within a view as it is simply not designed for that purpose.

Both Rails and .NET have concepts of *helpers* which are designed for particularly tricky design renderings such as a pagination item; or the expression of currency (say, you want positive integers wrapped in a span causing green lettering).  These small pieces of code will aid creating repeated complex design constructs throughout your site.

On a more specific scale, both frameworks provide capabilities for rendering what are termed *partials*.  If you've worked with a complex design before, you are probably quite familiar with the gigantic HTML soup that may result.  Partials allow the capability to break discrete and logical components up into re-usable components which can be embedded in many locations with a given application.

Rails and .NET also provide many built-in tools to aid with front-end design work, from asset pipelining to bundling which are outside the scope of a discussion on MVC style architecture.  I would, however, caution a reader to deeply consider their use in the modern climate.  Both JavaScript and HTML assembly in general are evolving at a rapid pace which far outstrips back-end frameworks' ability to keep up.  Built-in tooling often simply gets in the way of, and trips developers who are exclusively skilled in those arts.

# Modifications to the Pattern

As I mentioned above, MVC is but a pattern.  It is not a law or a complete axiom.  Its more like a guideline.  It provides for us a foundation on which we may build solid, repeatable and maintainable code - but it does not confine us simply to its walls.

The concept of a design pattern typically includes a specification of purpose.  Factories are great when you need several types of similar things (conforming to an interface, generally); Singletons are great when you need one and only one of an object; and MVC is great when you need to service a Web Request.

What this does *not* include though, are particularly complicated web applications in their entirety.  Ones which include many layers of logic that are distinctly separate and complex in their own rights.  It does not include complex image processing calculations or audio processing.  It does not include handling incredibly complex user authorization schemes or permissions hierarchies either.  It includes handling your every-day standard web request.

On more complicated applications, you are going to need to step outside of the MVC box and abstracting further components from your system into other discrete components in order to maintain cleanliness and understandability (not to mention, sometimes performance).

There are four common methodologies for separating out your application beyond what MVC provides:

1. Concerns or Decorators
1. Service Classes
1. Services
1. External Libraries

## Concerns or Decorators

Depending on your lingua franca, your general first choice may be Concerns or Decorators.  These are modules or classes which are, at their technical heart, used to extend the functionality of a class to which they are attached.  That sounds very concrete and real doesn't it?

In execution, these methodologies are used to break out and segregate similar pieces of logic.  Often times these are broken out to better organize code (a DeweyClassifiable concern/decorator which adds functionality to determine a book's Dewey Decimal Code for example) but can also be used to promote re-use of code across objects which share the same needs (a Book at a library or perhaps also a Video).

In the Rails world, the usage of Concerns is hotly debated.  Those in the 'against' camp typically speak to its unique solution to a problem which was typically solved using one of the other methods in the past.  It is also argued, that due to its lax implementations - it can lead to more difficult to understand spaghetti files.  In either case, if you lack prior examples in your code base - I urge the reader to try them and understand their nuances as most other Rails developers, whether they agree or not, understand their usage and will be able to grok your code.

## Service Classes

Service Classes typically represent classes, most often static, who's primary realm of responsibility lie with a particular domain use case.  They may also, especially in the case of .NET, be concerned with coordinating models with their persistence layer in addition to business concerns.  These are a useful method of segregating large pieces of related code into an easily found location and encourages separation of concerns.

In prior examples I discussed cakes:

>if an Icing Color is determined by the availability of 2 tubes of Royal Blue icing to be mixed with 1 tube of Scintillating Yellow or that it needs to be a working day and 2 days notice is required except before Christmas because there's loads of staff

In this example, you find a mixture of several domain concerns within the requirements statements.  In a project utilizing ServiceClasses, you may find an IcingService and a SchedulingService which are responsible for determining Icing availability and the ability to actually ice a cake separately.

In a Rails application, you may find this at odds with the concept of Concerns and rightly so.  They both solve roughly the same problem in different ways.  The merits in either are generally esoteric and boil down to a matter of preference.  Personally, in larger extremely complex applications, I find that Service Classes provide just the right level of abstraction and findability to allow future developers to easily navigate code-bases.

## Services

In extreme cases, you may find your ServiceClasses developing dependencies amongst themselves.  This is bad.  Generally speaking, your ServiceClasses should be *decoupled* and have little to no requirement of interaction, a Controller or other Coordinator class should be handling this if it is the case.

Alternatively, you may find the ServiceClasses which are becoming so inter-dependent solve a problem entirely on their own.  On deeper introspection, you may find that this problem actually expands beyond the scope of your software project and also solves problems that are being experienced within your immediate community (your organization, your development community, other developers who like burgers in your town).  At this point, you should begin considering breaking those classes out into their own Service project outright.

This project could then communicate with other projects, typically over an HTTP connection in order to exchange data.  Perhaps it is an Employee Scheduling Service that now your accounting team can integrate with in order to include scheduling in payroll forecasts.

This is not without its toils and perils and should not be undertaken lightly, but it can prove to be a boon if you already have or are looking to grow an interconnected infrastructure.

## External Libraries

As you are considering the Services option above, also consider if the data with which those interconnected ServiceClasses is what's valuable - or if its the action that's being performed.  Services, as above, most often benefit when they are working with a particular set of data and are able to act as a *canonical repository* for that data which other services can work with.

If it is more the way in which you are solving a problem, say adding a lens flair to an image, you may consider instead implementing a separate library.  In the case of Rails, this may manifest as a Gem or a Nuget package in .NET.  These provide an easy to install and self contained package other developers (or yourself) can leverage.

Depending on the sphere of influence in the problem that you are solving, however, you or your organization may wish to monetize this solution.  If, for example, while writing the next big social network you stumbled upon an incredible method of recognizing and announcing the food on the plates of socialites; you may wish to sell that technology in the future.  It is, arguably, far easier to monetize and control a Service based system than a Library based system.

# Conclusion

In general, MVC changed the face of web development.  As modern web developers, we use it every day even if we don't fully understand it.  I hope that this piece has helped you gain a deeper insight into your daily tool.

Keep writing great code.
