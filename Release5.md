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
* Scaphandre Prometheus Exporter has been added
* With `openstack-resource-manager` a new day 2 operations tool has been added.
* osism role for tuned to optimize system profiles is now present

### SCS Developer focused improvements (testbed and k8s cluster management)

## Upgrade/Migration notes

## Removals

## Deprecations

## Security Fixes

## Resolved Issues

## Standards Conformance

## Release Tagging

## List of known issues & restrictions in RX

## Contributing

We appreciate contribution to strategy and implementation, please join
our community -- or just leave input on the github issues and PRs.
Have a look at our [How to contribute page](https://scs.community/contribute/).

## Thanks
