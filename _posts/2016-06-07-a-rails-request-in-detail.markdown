---
title: A Rails Request in Detail
author: Jordan T. Norlen
date: 2016/06/07
summary: An in depth examination of what happens when you press enter in a Rails application
layout: post
---

# Introduction

What happens when you ask your browser for a web-page is a most curious culmination of hundreds of years of scientific, academic, government and business development.  At once, it is the simple act of retrieving an individual's Facebook page or Googling for how many legs a spider actually does have and at the same time, it is the result of hundreds of thousands of human beings, each standing atop each-other's shoulders to accomplish what would otherwise be impossible.

The unfathomable quantity of man-hours it took to reach a point; where from (roughly) anywhere in the world - you may simply hit "enter" on a keyboard, and learn what your friend halfway around the world had for breakfast, represents one of humanities great achievements.

The fundamental underpinnings of this incredible feat rely on a complex network of interconnected computer and telecommunications systems that ultimately result in the ability for a computer in one geographic location to easily converse with a computer in a completely separate geographic location.  Of course, what is communication with nothing to say?  The network infrastructure required to ferry messages back and forth is but a portion of the puzzle.  From there, a complex system of protocols and networking stacks ensure delivery of said message to waiting computer programs.

These computer programs themselves are complex in their own right, owing to the protocols that they were developed against, their users and the history which forged them.  They rely on languages and frameworks which have developed over a long and storied history which in turn rely on the actual software written to run on them.

This piece is intended to be consumed by a web application developer.  If you are looking for an exhaustive guide to the fundamentals of networking theory and TCP/IP - might I recommend the excellent Unix Network Programming book published by Addison-Wesley.  It, quite unexpectedly, has become my go-to reference for general computer network communications over the years.

# The Basic Infrastructure Flow

When you request a page in a web-browser, a number of agents step in to assist in handling your transaction.  Each section below is laid out from the perspective of each acting agent.

## Web Browser

Note that this excludes user interface actions (handling propagation of messages from the address bar input handler to the main action processing system for instance) as those vary widely and have little-to-no consequence for a web developer.

1. TCP/IP Socket Initiation (Handshake)
1. Optional Transport Encryption Begins
1. HTTP Request Delivery
1. HTTP Response Receipt
1. HTML Parsing and Cleanup
1. DOM Assembly and Loading
1. Socket Cleanup
1. External Resource Loading and Handling
1. JavaScript Run-Loop Begins
1. DOM Rendering

Note that, for a very long time now, browsers *stream* HTML into the DOM Assembly and Loading portions as they are received over the Socket Buffer.  At the same time, external resources are picked up and separate threads spawned to load and process *those*.  This can lead to some interesting load collisions if you become dependent on loading order. Ensure that dependencies are manually tracked and "on start" JS events tied to DOMContentLoaded or an equivalent based on your browser support level.

Put simply, HTML Parsing and Cleanup, Dom Assembly and Loading, JavaScript Run-Loop and External Resource Loading and Handling can all occur in parallel with an unpredictable order of completion.

## Mid-level Networking

1. Name Resolution
1. Route Determination
1. Route Establishment and Routing Table Caching
1. TCP/IP Socket Initiation (Handshake)
1. Message Delivery to Server
1. Message Receipt Confirmation from Server
1. Response Receipt from Server
1. Response Receipt Confirmation to Server
1. Socket Cleanup

## Load Balancer (if applicable)

1. Session Affinity Checks Regarding Assigned Nodes
2. Node Health Check Consultations
3. Node Ability to Satisfy (Round robin, load based or custom) Determined
4. Node Assignment for Request
5. Socket Initiation with Assigned Node (Handshake)
6. Optional Transport Encryption Begins with Proxied Service
6. Request Proxying Begins (X-HTTP-FORWARDED-FOR assigned and injected into contents if no encryption)
7. Response Streaming and Buffering
8. Response Delivery
9. Socket Cleanup

## Web Server (nginx, Apache, IIS, etc)

1. Connection Initiation over TCP/IP (Handshake)
2. Initial connection handshake and acknowledgement
3. Optional SSL exchange and envelope encryption begins
2. HTTP Request Processing
2. Worker Determination and Handoff
3. Basic Request Mapping
4. Reverse Proxy Handoff
5. Reverse Proxy Response Receipt
5. HTTP Response Delivery
6. Connection Cleanup

## Application Server (Passenger, Puma, Unicorn, etc)

1. FD or TCP/IP Message Exchange
2. Worker Determination and Handoff
3. Worker Response Receipt
4. Response Delivery

## Rails Application

1. Rack processes and transforms the raw HTTP Request
1. A database connection is withdrawn from the pool
1. Sessions are setup and configured in immediate memory for the scope
1. Routes are determined and chosen
1. Optional engines may be handed off to here
1. Relevant Controller is instantiated and their actions called
1. Custom code is executed and the result rendered through ActionView
1. Result is delivered back up through Rack, the Application Server, the HTTP Server, its established Socket and ultimately to the end-client

It bears noting that prior to a Rails web application's handling a request, it has generally been previously initialized into an Application Pool by the Application Server.  This process generally involves:

1. Bootstrapping Rails and custom code into memory
2. Reading and storing environment variables based on the parent process and what passed during forking
3. Database connection pool initialized
3. Initializers Ran (global, then environment, then initializers/\*)

It also bears noting that Rails Applications, when ran in production mode, cache their Ruby code in memory to enhance execution time.  This means that the system can avoid a trip to the (relatively) slow hard disk in exchange for higher memory requirements and the need to restart an application on change.

## Optional Transport Encryption

1. TCP/IP Connection Initiation over Port 443
2. SSL or TLS Version Determination Between Systems (Handshake)
3. Certificate Receipt from Server
4. Certificate Validation with pre-programmed Certificate Authorities on Client System (unless otherwise disabled)
5. Amazing Cryptography Happens that I Don't Understand
1. Message Exchange and Secure Channel Communication Proceeds
6. TCP/IP Connection Termination

# But What About Inside an Application?

So we've talked about what happens on an infrastructure level.  Let's talk about what happens inside an actual Rails application.  As an example, let us consider a GET request to `https://gitlab.com/gitlab-org/gitlab-ce`.  For these purposes, we will forego the infrastructure discussion and dig straight into the custom Rails application.

*You can follow along with the code from https://gitlab.com/gitlab-org/gitlab-ce/tree/8-9-stable as of July 7th, 2016*

*Note, this is generated from a cursory exploration of the afore-mentioned source code.  More in depth study would be required to fully articulate and validate the steps presented below.*

## Routing

If we take a look at the `config/routes.rb` file, we can see that there is a section around line 464 headed up as "Project Area".  All other previous routes failing, due to the extensively low specificity of the path (/), this route will be chosen by the processor.

We can see that it contains constraints for particular ids, which limit roughly to a humanized name, further limiting the scope as well as a definition that it has a sub-resource called projects.

Thus, the parser will first match a namespace (Organization in GitHub lingo) and then attempt to load in the ProjectsController for that nested resource.

Consequently, we could explore /gitlab-org in order to cause the routing engine to choose to execute the namespaces_controller instead.  In this instance, however, we have grabbed the nested ProjectsController route and have included the id (gitlab-ce) in the parameterized call to the ActionController.

## Controller Processing Setup

We can now move on to examining the projects_controller.rb to continue our exploration of what happens when we hit enter.

Our first encounter here is that the ProjectsController inherits from its parent Projects::ApplicationController (line 1) which means we should also pull that file up for consultation.

Prior to executing the Controller's actions, ActionController will first process and execute the before_action files as they apply to a given request.  Since in our case we have issued a GET for the show action, we will be executing :project, :repository, :assign_ref_vars and :event_filter.

Most of these actions, you will find defined on the derived controller's (ProjectsController) parent Projects::ApplicationController.  You will note, however, that the :assign_ref_vars is actually included from the ExtractsPath module which will add the desired functionality.

In essence, we are:

1. Loading and authorizing access to the @project (with some custom MySql specific ordering if you dig down into project.rb)
1. Loading and authorizing access to the @repository variable *from* the @project
1. Assigning a load of common variables for this type of view
1. Assigning filters to what appears to be an event engine
    1. This portion of the code is uncommented, and its use unapparent from initial inspection.

But wait!  You say, event_filter wasn't defined in any of those files!  That's right.  Projects::ApplicationController inherits from the global ApplicationController; which means that class is ALSO brought into the mix.  It brings in its own collection of various before_action methods which are called prior to execution.

One more thing, this controller uses a custom method to determine which layout will be rendered.  You can see this on line 13 of the code and should be aware when attempting to track down any particular FE bugs.

## Action Processing

Ah, the meat.  At this point, with the before_action filters satisfied - we begin analyzing the execution of the `def show` method found on line 92.  We can see some basic sanity checks are enacted to see if the repository is undergoing some long-running task (import or deletion).

We can also see that the system is designed to not only present and HTML view, but also an Atom feed if the requestor has included such in its headers.  This respond_to block is an example of Rails being friendly towards multiple formats.  Depending upon the X-ACCEPTS headers passed, Rails will execute the specified blocks.  We will continue to focus on the HTML view.

We can now see that we load the user's notification_settings (presumably to control alert displays) and then run further sanity checks on the project before yielding it up to the view.

At this point, the `app/views/projects/show.haml` template is parsed, assembled and rendered.  The controller, now having rendered the view, returns it up the chain to be sent back to the client's browser for rendering.

## The Model Layer

Earlier on we described the loading of the @project variable.  We can dig further into project.rb to find the correlated `find_with_namespace` (line 264) which is responsible for that.  It assembles a combination of namespace and project path and then runs it through its base ActiveRecord method of .quote in order to ensure sql injection prevention.

Why?  You ask?  The assembled path (which potentially includes user input) is the injected into a (potentially) unprotected SQL string used for ordering the results.  We then find a call to a deeper query assembly method responsible for cleaning up the pathing, query and assembling a more robust search string.

The previously assembled order string is used to reorder the results and the first result returned by `take`.

## The Browser

Once the Controller has done its duties and handed its results fully up the communications chain, the user's web browser takes over.  As described above, it will begin streaming contents as they arrive and loading externally referred resources as needed.  These may be images, stylesheets or JavaScript files.  From here, a world of user interactions may take place and yet another book could be composed with the goings on of various components and paths available to the user now.  Ultimately though, the user is finally viewing the intended data.

## Encryption Please?

Since our request was explicitly for https, our browser autoselected and used an appropriate SSL/TLS encryption scheme based on our infrastructure examination above.  You will note, accessing the same URI via HTTP will automatically redirect the user to HTTPS.  I could find no evidence in the included code (though a thorough dive was not performed) of an enabled application level SSL redirect, and suspect this is being performed on the HTTP server itself.  This is particularly useful when developing applications requiring SSL, as you can easily test locally without the need to setting up a full web server stack.

## Rails Recap

The above description is the tip of the iceberg for what's really going on behind the scenes.  The above controller inherits (by way of its own parent) the ApplicationController which contains many more constraints and actions used to ferry data between the Model layer and the subsequent View layer.  DoorKeeper and Devise are also in play for handling authorization as well as several other layers (including an LDAP integration).  Each of these additional gems carry with them their own execution paths that will not be examined in this text.

We can see above how MVC begins to show its hand as well through the allowing of the controller to be concerned with handling the request itself and the user's authentication and authorization, meanwhile the Model layer handling the business end of actually loading, hydrating and ordering projects before setup.

# Conclusion

The previously described sections barely scratch the surface of what actually goes on with a web request.  There are a gigantic quantity of particulars that could be expanded from ranging from:

1. How TCP/IP envelopes are constructed
1. How UDP interplays with TCP/IP during a DNS request
1. How DNS' hierarchy allows for a local server to "know" about the entire Internet
1. How Web Servers themselves may handle load balancing
1. Where caching proxies fit in
1. How CDNs may be used when serving static or mostly-static files
1. How ARP is utilized to advertise, request and cache network addresses
1. ARP's interplay with routing protocols
1. Routing protocols!

and a whole lot more.  I hope that the foregoing text has opened your eyes to an immensely interesting and ever-expanding universe of how your web application is allowed to actually function on the Internet.
