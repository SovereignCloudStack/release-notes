# Releases and Roadmap

## Release 0 (2021-07-15)

SCS R0 has been released on 2021-07-15 and bundles the work
accomplished by the community prior to the full start of the project.

See [Release Notes for R0](Release0.md) for more information.

## Release 1 (2021-09-29)

R1 came quickly after R0 and was the first release to ship a production ready k8s stack
(with k8s cluster API), some identity federation integration and much improved
preconfiguration for monitoring and logging.

See [Release Notes for R1](Release1.md) for more information.

## Release 2 (2022-03-23)

This release delivers vast improvements for bare metal automation
and the features in the container layers.

See [Release Notes for R2](Release2.md) for more information.

## Release 3 (2022-09-21)

Release 3 features user federation, increase in deployment and upgrade
velocity by improving automated test coverage as well as bringing disk encryption
based on tang from the state of a technical preview to be fully supported.

See [Release Notes for R3](Release3.md) for more information.

### Release 4 (2023-03-22)

The implemented open source components have been updated to the latest stable versions.
Among others, this includes OpenStack Zed, Kubernetes Cluster API 1.3.x, Cluster API Provider
for OpenStack 0.7.x, Kubernetes 1.26.x, and Ubuntu 22.04 LTS.

See [Release Notes for R4](Release4.md) for more information.

### Release 5 (2023-09-20)

No stone was left unturned in this release. As such the list of highlights is long, but
does include OpenStack 2023.1 (Antelope), Ceph Quincy, Cluster Stacks Technical Preview,
Support for diskless flavors, and IPv6 support for east-west and north-south.

See [Release Notes for R5](Release5.md) for more information.

### Release 6 (2023-03-20)

Along the outcomes that were defined for the R6 development cycle many things have been
added, improved and fixed through the last six months. The list of highlights is lead by
the release of Cluster-Stacks, the new KaaS observability stack as well as the IaaS
reference implementation being updated to OpenStack 2023.2 (Bobcat).

See [Release Notes for R6](Release6.md) for more information.

## Roadmap

We have a 6 month release cadence -- R7 will follow in September 2024.
Until then, we will provide bugfixes and security fixes for R6.

We do work towards a model where our partners can actually follow our main
development branches -- right now, our CI needs a bit more coverage though
to make this safe.
