==========================
Architecture Working Group
==========================

Mission
=======

    Be the recognized forum of expertise on the design and architecture of
    OpenStack and provide guidance and resources to the Technical Committee
    and the entire OpenStack community on architectural matters.

This group is be open to all volunteers who are interested in participating
in OpenStack-wide design discussions.

Also note that this is an advisory group intended to gather information
and produce recommendations.  No new authority for enforcing those is
to be conferred onto this group, however accepted recommendations will
be expected to be followed for new work whenever possible.

Scope
=====

* Document the existing architecture and relationship of OpenStack projects.
* Provide information and guidance to the TC and the OpenStack community on
  architectural matters.
* Provide productive spaces for architects to share their designs and gain
  support across projects to move forward with, coordinate, and implement
  architectural decisions.
* Start with a limited scope in order to refine the group processes and be
  able to achieve visible results (positive or negative) within the upcoming
  Ocata release cycle.

Communication
=============

* Email: `openstack-dev mailing list`
* IRC: #openstack-meeting-alt on Freenode
* Meetings: `OpenStack wiki`_, `weekly meeting schedule`_ (includes day/time,
  logs, ICS file, agenda)

.. _openstack-dev mailing list: http://lists.openstack.org/cgi-bin/mailman/listinfo/openstack-dev
.. _OpenStack wiki: https://wiki.openstack.org/wiki/Meetings/Arch-WG
.. _weekly meeting schedule: http://eavesdrop.openstack.org/#Architecture_Working_Group

Deliverables
============

* [details tbd]

How To Contribute
=================

Topics
------

The WG maintains a backlog of topics in the repo under ``backlog/``.  Additions
to the backlog should be proposed using Gerrit.  Approval for addition is
concerned primarily with the proposal fitting into the scope of the WG.

* A topic background document should be proposed to the WG repo under backlog/
* The proposal will be held for review for a minimum of one week to allow time
  for dicussion regarding its fitness for the scope of the WG.
* The proposal should also be added to an upcoming IRC `meeting agenda`_ to
  allow the proposer to make a pitch and answer questions.
* Proposals that do not receive objections after the review period will be
  considered approved as in-scope and merged into the backlog.
* The WG will move proposals from the backlog to the ``active/`` work area as
  it chooses to work on them.

The background document should be sufficient to describe the topic and use
cases or other reasons it should be considered, such as how it would improve
OpenStack as a whole or solve specific use cases.

.. _meeting agenda: https://wiki.openstack.org/wiki/Meetings/Arch-WG

Participation
-------------

Participation in the Architecture Working Group is voluntary and defined by
those who attend the meetings and contribute proposals, reviews and effort into
the group activities.  There is a core review group (seeded with volunteers at
early meetings) that has the ability to commit changes to the WG Git repo. As
with most of OpenStack, membership of that group is merit-based with
participation and other contributions being considered.

Background
==========

OpenStack is a big system. We have debated_ what it actually is,
and there are even t-shirts to poke fun at the fact that we don't have
good answers.

But this isn't what any of us wants. We'd like to be able to point at
something and proudly tell people "This is what we designed and implemented."

.. _debated: http://lists.openstack.org/pipermail/openstack-dev/2016-May/095452.html

Problem description
-------------------

For each individual project, design is a possibility. Neutron can tell you
they designed how their agents and drivers work. Nova can tell you that they
designed the way conductors handle communication with API nodes and compute
nodes. But when we start talking about how they interact with each other,
it's clearly just a coincidental mash of de-facto standards and specs that
don't help anyone make decisions when debugging, refactoring, or adding on
to the system.

Oslo and cross-project initiatives have brought some peace and order to the
implementation and engineering processes, but not to the design process. New
ideas still start largely in the project where they are needed most, and often
conflict with similar decisions and ideas in other projects , including dlm,
taskflow, tooz, service discovery, state machines, glance tasks, messaging
patterns, database patterns, etc. etc. Often this creates a log jam where
none of the projects adopt a solution that would align with others. Most
of the time when things finally come to a head these things get done in a
piecemeal fashion, where it's half done here, 1/3 over there, 1/4 there,
and 3/4 over there..., which to the outside looks like **architecture chaos**,
because that's precisely what it is.

And this isn't always a technical design problem. OpenStack, for instance,
isn't really a micro service architecture. Of course, it might look
like on the diagram below, but we all know it really isn't.

.. image:: http://docs.openstack.org/admin-guide/_images/openstack-arch-kilo-logical-v1.png
   :width: 650px

The compute node is home to agents for every single concern, and the API
interactions between the services is too tightly woven to consider many
of them functional without the same lockstep version of other services
together. A game to play is ask yourself what would happen if a service
was isolated on its own island? How functional would its API be, if at
all? Is this something that we want? No. But there doesn't seem to be
a place where we can go to actually design, discuss, debate, and build
consensus around changes that would help us get to the point of gathering
the necessary will and capability to enact these efforts.

The need for attention ranges from the above project interaction level up
to higher-level questions such as 'What other OpenStack services can any
particular service assume to be available?'
