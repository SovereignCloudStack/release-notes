# Release Notes for SCS Release 5
(Release Date: 2023-09-20)

This document is work in progress for the upcoming Release 5.
This note will be removed, once Release 5 is released and these notes are valid.

## Scope

Just as our previous release, Release 5 has been developed alongside a set of associated outcomes.
These outcomes are comprised of:

* SCS is standardized
* SCS is understandable
* SCS is transparent
* SCS is continuously built and tested
* SCS is opinionated
* SCS enables

## Component Versions and User-visible improvements (highlights)

* The IaaS referencen implementation is based on [OSISM 6.0.0](https://release.osism.tech/notes/6.0.0.html).
* [OpenStack 2023.1 (Antelope)](https://releases.openstack.org/antelope/highlights.html)
* Default Ceph version is now [Ceph Quincy](https://docs.ceph.com/en/reef/releases/quincy/#v17-2-5-quincy)
* OVN and OVS have been updated to their latest versions (OVN: 23.06.1, OVS: 3.2.0)
* IPv6 east-west and north-south support

#### Observability


#### Systems management

With the `openstack-resource-manager` a new day 2 operations tool has been added.
Furthermore a osism role for tuned to optimize system profiles is now present.


### Operator focused improvements

* The [openstack-flavor-manager](https://github.com/osism/openstack-flavor-manager) is now able to create all standard, mandatory SCS flavors for you.
* [Aptly](https://github.com/osism/helm-charts/tree/gh-pages/aptly) mirror setup (for deb packages) is now available to be deployed alongside OSISM installations. This requires additional infrastructure. Have a look at the [documentation](https://github.com/osism/docs/tree/main/docs/operations/external_services/aptly_external.md) for more details.
* Scaphandre Prometheus Exporter has been added to export power consumption metrics more easily.
* With `openstack-resource-manager` a new day 2 operations tool has been added.
* To optimize system profiles an osism role for tuned is now present.

### SCS Developer focused improvements (testbed and k8s cluster management)

## Upgrade/Migration notes

* For the IaaS reference implementation, please refer to the [OSISM 6.0.0 Upgrade Notes](https://release.osism.tech/notes/6.0.0.html#upgrade-notes).

## Removals

* Please check the removals for OSISM in the [upstream removal notices](https://release.osism.tech/notes/6.0.0.html#removals).

* The services `minio.services.osism.tech` and `harbor.services.osism.tech` are deprecated and will be turned of on october 20th, 2023.

## Deprecations

### Deprecations via OSISM

For these please also refer to the [upstream deprecation notices](https://release.osism.tech/notes/6.0.0.html#deprecations).

* It is again noted that the old scripts of the form ``osism-`` will be removed in the future.
  A note has been added to the scripts showing this when they are executed.

## Security Fixes

Throughout the Release 5 development cycle, the SCS project issued two security advisories for upstream components:

* In April 2023 an advisory in Open vSwitch (OvS) ([CVE-2023-1668](https://cve.report/CVE-2023-1668) was issued.
Our [advisory](https://scs.community/security/2023/04/21/cve-2023-1668/).

* In May 2023 an advisory affecting the OpenStack component Cinder ([CVE-2023-2088](https://cve.report/CVE-2023-2088)) was issued.
Our [advisory](https://scs.community/security/2023/05/10/cve-2023-2088/).


## Resolved Issues

## Standards Conformance

## Release Tagging

## List of known issues & restrictions in R5

## Contributing

We appreciate contribution to strategy and implementation, please join
our community -- or just leave input on the github issues and PRs.
Have a look at our [How to contribute page](https://scs.community/contribute/).

## Thanks