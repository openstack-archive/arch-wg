=============
Base services
=============


Definition
==========

Components of OpenStack do not run in a vacuum. They leverage features present
in a number of external services to run. Some of those dependencies are local
(like a hypervisor on a compute node), while some of those are global (like
a database). "Base services" are those global services that an OpenStack
component can assume will be present in an "OpenStack" installation, and that
they can therefore leverage to deliver features. Components of course do not
*have to* use those, but they can.


Analysis and options
====================

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


Current base services
---------------------

There are currently three "base services" that components can assume will be
present in an "OpenStack" installation:

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


Proposed process for addition of new base services
--------------------------------------------------

The decision to add new base services to OpenStack impacts all of OpenStack.
The Architecture WG therefore suggests that such additions are ultimately
approved by the OpenStack Technical Committee. When new base services are
proposed, the Architecture WG proposes to provide a thorough analysis to
weigh the trade-off mentioned above. Based on that analysis, the Architecture
WG will make a recommendation to the TC (which the TC is free to follow or
ignore). The list of available base services will be maintained as a TC
governance document under the openstack/governance repository (and published
on the governance.openstack.org website).
