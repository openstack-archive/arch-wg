Introduction
============

In the beginning there was Nova. It included volumes, networking,
hypervisors, and scheduling.  Since then, Nova components have either
been replaced (nova-network with Neutron) or forklifted out and enhanced
(Cinder). In so doing, interfaces were defined for how Nova would
continue to make use of these now-external services, but nova-compute,
the place where the proverbial rubber meets the road, was left inside
Nova. This meant that agents for Cinder and Neutron had to interact with
nova-compute through the high level message bus, despite being right
on the same physical machine in many (but not all) cases. Likewise,
some cases take advantage of that, and require operator cooperation in
configuring for certain drivers.

This has led to implementation details leaking all over the API's that
these services use to interact. Neutron and Nova do a sort of haphazard
dance to plug ports in, and Cinder has drivers which require locking files
on the local filesystem a certain way. These implementation details are
leaking into public API's because it turns out nova-compute is actually
a shared service that should not belong to any of the three services,
and which should define a more clear API which Nova, Cinder, and Neutron,
should be able to use to access the physical resources of machines from
an equal footing.

proposed next steps
===================

 * Produce an accurate analysis on the current state of nova-compute's
   interaction with other OpenStack services.

 * Produce a cross-project spec for moving nova-compute out of Nova
   and defining an API for it that meets the needs of all other projects.
