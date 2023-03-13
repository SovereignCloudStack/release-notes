# Release Notes for SCS Release 4

This document is work in progress for the upcoming Release 4.
Release 4 will be released on March 22nd, 2023. 
This note will be removed, once Release 4 is released and these notes are valid.


## Scope

Release 4 has been developed alongside a set of associated outcomes. These outcomes comprise of:

* SCS is standardized
* SCS is federated
* SCS is continuously built and tested
* SCS is understandable
* SCS enables Operators with an excellent toolbox

The SCS project is completly developed in the open, based on the principles of the four opens. Due to this a lot of our work can be tracked and used continously without waiting for the half-year releases. Especially, but not limited to, this included our efforts in regards to documentation and our standards.

One of the major highlights that happened in the R4 development cycle is our work on assuring _SCS is understandable_.
Be sure to look at [our new documentation entry point](https://docs.scs.community).

## Component Versions and User-visible improvements (highlights)

* [OpenStack Zed release](https://releases.openstack.org/zed/highlights.html)
* Ceph Quincy is available, the default release of Ceph is still Pacific.
* The base infrastructure is provided by
  [OSISM 5.0.0](https://release.osism.tech/notes/5.0.0.html)
  which in turn builds on top of kolla and kolla-ansible.
* With [osism/node-image](https://github.com/osism/node-image) an iso image for much easier bootstrapping of new OSISM environments is available now
* The Kubernetes CAPI images have been upgraded from Ubuntu 20.04 to Ubuntu 22.04.

## New Features (Highlights)

### Operator focused improvements

* The [Openstack Image Manager](https://github.com/osism/openstack-image-manager) has seen many improvements and is the reference command to assure the images available comply with the [SCS Image Standard]( ) // FIXME LINK
* For Ceph, special playbooks were added to validate the deployment status of the OSD, MON and MGR services in OSISM. The commands for use are `osism validate ceph-osds`, `osism validate ceph-mons`, and `osism validate ceph-mgrs`.

### SCS Developer focused improvements (testbed and k8s cluster management)

* The testbed has been significantly simplified for new operators and developers and a [Quick Start](https://docs.osism.tech/testbed/quickstart.html) guide has been added


## Upgrade/Migration notes

## Removals

## Deprecations

## Security Fixes

Throughout the Release 4 development cycle, the SCS project issued two security advisories for upstream components:

* In November 2022 an advisory regarding [CVE-2022-3602 and CVE-2022-3786](https://www.openssl.org/news/secadv/20221101.txt) in OpenSSL was issued.
Our [advisory](https://scs.community/security/2022/11/01/advisory-spookyssl/).

* In February 2023 an advisory regarding [CVE 2022-47951](https://cve.report/CVE-2022-47951) in OpenStack components nova and glance was published.
Our [advisory](https://scs.community/security/2023/01/24/cve-2022-47951/).

## Resolved Issues

## Standards Conformance

## Release Tagging

## List of known issues & restrictions in R4

## Contributing

We appreciate contribution to strategy and implementation, please join
our community -- or just leave input on the github issues and PRs.
Have a look at our [How to contribute page](https://scs.community/contribute/).

## Thanks

The work for R4 has been done by many contributors from our community.
The special thanks goes out to our contributors who participate in our community
on a very regular base - without these the various team calls and events like
the hackathons would be much less successful and fun.

Of course we are leveraging a huge amount of open source technology that has been
created by our friends in other communities, many of which are part of the
CNCF, Linux Foudation, OIF, and others. We participate and contribute where
we can and definitely want to acknowledge the great work that we build upon.
