=============
Base services
=============


Summary
=======

Components of OpenStack do not run in a vacuum. They leverage features present
in a number of other services to run. Some of those dependencies are local
(like a hypervisor on a compute node), while some of those are global (like
a database). "Base services" are those *global* services that an OpenStack
component can assume will be present in an "OpenStack" installation, and that
they can therefore leverage to deliver features. Components of course do not
*have to* use those, but they can assume they will be available, in case they
*want* to use them.


Rationale
=========

In the early days of OpenStack, we decided (without really writing it down)
that every OpenStack component could assume that a number of base services
would be available: a relational database (MySQL), a message queue (RabbitMQ),
and an AuthN/AuthZ token service (Keystone). Being able to assume that those
external services would be present encouraged components to make use of them,
and also encouraged convergence on the same set of components, reducing
operational impact.

That early decision served us well, but since then we were unable to grow
that set of base services. Some of it is due to being extra-careful not to add
new mandatory services that would impact all OpenStack deployments, some of it
is due to not defining the base services concept in the first place, and some
of it is due to the growth of OpenStack and the difficulty to make such central
decisions.

However, we still very much need to build on top of advanced base features,
like a distributed lock manager (Zookeeper?), or a secrets vault (Barbican?).
Rather than making the hard decision, we work around their absence in every
project, badly emulating those features using what we have, or reinventing
them locally. In the name of containing overall operational complexity, we
make every component more monolithic and less efficient, increasing operational
complexity.

Recognizing "base services" for what they are, and having a path to grow the
set of those base services, is therefore essential to the continued growth
and relevance of OpenStack. This is what this proposal is addressing.


Detailed analysis
=================

The trade-off with base services
--------------------------------

Leveraging features from a base service (rather than working around
limitations or badly reinventing the wheel) is key to reaching acceptable
levels of stability, performance and scaling. There is therefore a natural
tendancy for developers to add the right tool for the right job whenever
it's needed. However, since they will likely have to be deployed in most
OpenStack deployments, base services increase the operational complexity
of running OpenStack.

So it is very important to balance those two sides and very conservatively
consider proposed additions to the base services list, especially when those
additions introduce a whole new class of operational challenges. It is also
very important to consider how performant, stable and secure the new
dependency is: since a complex system is only as performant, stable or
secure as its weakest link, additional services have the potential to
adversely impact the system if they are not up to par with the rest of the
base services.

Narrow or wide base services
----------------------------

There are two possible approaches with base services. They can be narrow,
and focus on a particular implementation. Or they can be wide, using an
indirection layer (generally an interface library) below which multiple
competing solutions could be plugged. With the narrow approach, you can
take advantage of the unique features of a particular solution, you don't
have to limit yourself to some common denominator between multiple solutions.
It is also less costly from a development standpoint, with only one interface
layer to maintain and test. The wide approach is more operator-friendly: it
lets deployers choose their preferred underlying implementation.

Historically, OpenStack has opted for the wide approach, generally supporting
as many underlying solutions as possible. Lately, a "narrow wide" approach
was followed: while we still use indirection layers and support multiple
options, we try to limit the number of options available. Only tested,
featureful options are supported in mainline code, and only to the point
where they provide useful choice. As a result of this, untested or
non-functional or useless options are getting pruned, to focus on a
limited set of options. That gives deployers some choice while keeping
development cost/distraction under control.


Proposal
========

This proposal is limited to defining the concept, specifying the current list
and writing a process to grow the list. It is specifically *not* proposing
any specific addition.

Definition of base services
---------------------------

Base services are services that OpenStack components can assume will be
present in any OpenStack deployment. OpenStack components may therefore
leverage advanced features exposed by those base services without fear of
increasing the overall operational complexity for OpenStack deployers.

Current list of base services
-----------------------------

There are currently three base services that components can assume will be
present in any "OpenStack" installation:

* An oslo.db-compatible database (MySQL)
  OpenStack components store data in a database, using oslo.db as an
  indirection layer. While most OpenStack deployments use MySQL, other
  databases are supported.

* An oslo.messaging-compatible message queue (RabbitMQ)
  Some inter-process and inter-service communication in OpenStack
  components is accomplished using message queues, through oslo.messaging
  as an indirection layer. While most OpenStack deployments use RabbitMQ,
  other message queues are supported.

* Keystone
  OpenStack Keystone handles AuthN/AuthZ for OpenStack components.
  Deployments can assume that Keystone will be present to perform that role.

The list of available base services will be maintained as a living TC
governance document under the openstack/governance repository (and
published on the https://governance.openstack.org/tc website).

Process for addition of new base services
-----------------------------------------

The decision to add new base services to OpenStack impacts all of OpenStack,
therefore such additions should be ultimately approved by the OpenStack
Technical Committee. When new base services are proposed, the Architecture WG
provides a thorough analysis to weigh the trade-off mentioned above. Based on
that analysis, the Architecture WG produces a recommendation to the TC (which
the TC is free to follow or ignore).


Implementation
==============

Assignee: Thierry Carrez (ttx)

Work items
----------

 * Propose concept definition and current list to the governance repository
 * Get Technical Committee approval on those documents
 * Add mentions of "base services" in the Project Team Guide

