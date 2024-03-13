# Release Notes for SCS Release 6	

This document is work in progress for the upcoming Release 6.
Release 6 will be released in March/2024. 
This note will be removed, once Release 6 is released and these notes are valid.


## Scope

Just as our previous release, Release 6 has been developed alongside a set of [associated outcomes](https://scs.community/2023/12/29/scs-r6-enables/).
Overall with Release 6 it becomes apparent that SCS is efficient to operate.

* SCS is standardized
* SCS is understandable
* SCS enables
* SCS is transparent
* SCS is continuously built and tested
* SCS is opinionated
* SCS charters new territory


## Component Versions and User-visible improvements (highlights)

### IaaS

The IaaS reference implementation is based on [OSISM 7.0.0](https://release.osism.tech/notes/7.0.0.html)
and delivers the following components:

* [OpenStack 2023.2 (Bobcat)](https://releases.openstack.org/bobcat/highlights.html)
* Default Ceph version is [Ceph Quincy](https://docs.ceph.com/en/reef/releases/quincy/#v17-2-5-quincy)
* [Ceph Reef](https://docs.ceph.com/en/latest/releases/reef/) is included as a technical preview
* [OVS](https://www.openvswitch.org) and [OVN](https://www.ovn.org/en/) have been updated and switched to LTS versions
  * [OVS 3.3.0](https://mail.openvswitch.org/pipermail/ovs-announce/2024-February/000343.html)
  * [OVN 24.03.0](https://mail.openvswitch.org/pipermail/ovs-announce/2024-March/000344.html)

### KaaS

* [k8s-cluster-api-provider](https://github.com/SovereignCloudStack/k8s-cluster-api-provider) (KaaS v1)
  * Moved to OpenTofu
  * Migrated to ClusterClass
  * HTTP_Proxy configurable
  * OVN LB
* [Cluster Stacks](https://github.com/SovereignCloudStack/cluster-stacks) (KaaS v2)
  * First Cluster Stacks for OpenStack build
  * [Cluster Stack Operator](https://github.com/SovereignCloudStack/cluster-stack-operator/)
    * core component to manage Cluster Stack releases 
  * [Cluster Stack Provider OpenStack](https://github.com/SovereignCloudStack/cluster-stack-provider-openstack)
    * manages node image release for OpenStack
  * [csctl](https://github.com/SovereignCloudStack/csctl)
    * builds Cluster Stacks assets
    * builds node images
    * tests Cluster Stacks

## New Features (Highlights)

### Operator focused improvements

### SCS Developer focused improvements (testbed and k8s cluster management)

#### KaaS
* Every component of Cluster Stacks brings a Tilt environment for local test and development
* With csctl Cluster Stacks assets can be created and tested locally without uploading to GitHub

### Preview: Domain Manager Persona

A Domain Manager role has been established in a [standard draft in SCS](https://docs.scs.community/standards/scs-0302-v1-domain-manager-role) aiming to allow self-service capabilities for customers at the identity API.
Work is in progress to [contribute this functionality to OpenStack Keystone](https://bugs.launchpad.net/keystone/+bug/2045974) and the corresponding [upstream spec](https://review.opendev.org/c/openstack/keystone-specs/+/903172) is currently under review.
The feature is expected to be available in the next SCS release.

## Upgrade/Migration notes

* For the IaaS reference implementation, please refer to the [OSISM 7.0.0 Upgrade Notes](https://release.osism.tech/notes/7.0.0.html#upgrade-notes).

## Removals

* Please check the removals for OSISM in the [upstream removal notices](https://release.osism.tech/notes/7.0.0.html#removals).

## Deprecations

## Security Fixes

## Resolved Issues

## Standards Conformance

## Release Tagging

## List of known issues & restrictions in R6

### KaaS
* Some features of KaaS v1 are not available yet in KaaS v2 because they are WIP in upstream CAPO

## Contributing

We appreciate contribution to strategy and implementation, please join
our community -- or just leave input on the github issues and PRs.
Have a look at our [How to contribute page](https://scs.community/contribute/).

## Thanks

