# Release Notes for SCS Release 5
(Release Date: 2023-09-20)

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

### IaaS

* The IaaS reference implementation is based on [OSISM 6.0.0](https://release.osism.tech/notes/6.0.0.html).
* [OpenStack 2023.1 (Antelope)](https://releases.openstack.org/antelope/highlights.html)
* Default Ceph version is now [Ceph Quincy](https://docs.ceph.com/en/reef/releases/quincy/#v17-2-5-quincy).
* OVN and OVS have been updated to their latest versions (OVN: 23.06.1, OVS: 3.2.0).
* IPv6 east-west and north-south support is present and documented upstream.
* [Cloud-in-a-Box](https://github.com/osism/cloud-in-a-box) now comes with Swift enabled as well as the option
for secondary NIC for external connectivity.

### Container Management

* The Kubernetes Cluster Management solution is [available as version 6.0.0](https://github.com/SovereignCloudStack/k8s-cluster-api-provider/blob/main/Release-Notes-R5.md)
* [Kubernetes](https://github.com/kubernetes/kubernetes) v1.24 .. [1.27](https://github.com/kubernetes/kubernetes/releases/tag/v1.27.6) are officially supported. [v1.28](https://github.com/kubernetes/kubernetes/releases/tag/v1.28.2) also works (technical preview until officially supported by capo) as do older versions (with downgrading nginx-ingress), matching OCCM and CSI versions.
* Cluster-API (capi) v1.5.1, Cluster-API provider for Openstack (capo) v0.7.3 
* The node images now use Ubuntu 22.04, the management host can use Ubuntu 22.04 or Debian 12.
* Cilium v1.14.1, default now, though Calico (3.26.x) is still supported.
* Cilium also brings the upcoming gateway API (opt-in) as technical preview.
* The Harbor container registry can now be rolled out with each cluster.
* The clusters can use a registry as cache to upstream dockerhub or gcr registries.
* The cluster management now works also on OpenStack clouds with a custom CA.
* Storage snapshots are supported now (fix was also backported to maintained branches).
* Diskless flavors are supported everywhere (cluster-management, health-monitor).
* etcd defragmentation and backup.
* Controls for pod and service IP ranges.

### Preview: Cluster-Stacks
The old scripts that are used to create, change and delete Kubernetes clusters with
Cluster API will be replaced by a proper Operator in the next release. A description can be found at the
[cluster-stacks](https://github.com/SovereignCloudStack/cluster-stacks)
and [cluster-stack-operator](https://github.com/SovereignCloudStack/cluster-stack-operator)
repositories. The technical preview can be tried with the [cluster-stacks-demo](https://github.com/SovereignCloudStack/cluster-stacks-demo).
This solution will fit more nicely into the CNCF landscape and
also allow for easier support of IaaS solutions that do not comply to our SCS
IaaS standards.

### Operations and IAM related

* A number of improvements when using identity federation via OIDC has been added, including
  addressing openstack CLI usage with PKCE Device Authz Grant, logout, and the usage of a
  proxy realm in keycloak. Improvements have been contributed to upstream keystone.
* With the `openstack-resource-manager` a new day 2 operations tool has been added.
  Furthermore an osism role for tuned to optimize system profiles is now present.
* The [openstack-flavor-manager](https://github.com/osism/openstack-flavor-manager) is now able to create all standard, mandatory SCS flavors for you.
* Scaphandre Prometheus Exporter has been added to export power consumption metrics more easily.
* To optimize system profiles an osism role for tuned is now present.
* Full support for air-gapped installation and operation of environments.
* A migration script and guide for moving from R4 to R5 clusters is available.
* Metering has been improved and a reference billing API implementation is available as technical preview.

### SCS Developer focused improvements (Cloud-in-a-Box, testbed and k8s cluster management)

* Documentation on testbed and Cloud-in-a-Box have been reworked.
* Reflecting CiaB's usage as edge cloud appliance, it now receives more automated testing.

### Project Infrastructure
* zuul.scs.community now complements OSISM's existing zuul infrastructure and is used also
  by the container layer to execute the CNCF e2e tests.
* registry.scs.community has been migrated to a new IaaS location (documented in a blog
  article) and is kept up-to-date now.

## Upgrade/Migration notes

* For the IaaS reference implementation, please refer to the [OSISM 6.0.0 Upgrade Notes](https://release.osism.tech/notes/6.0.0.html#upgrade-notes).

## Removals

* Please check the removals for OSISM in the [upstream removal notices](https://release.osism.tech/notes/6.0.0.html#removals).

* The services `minio.services.osism.tech` and `harbor.services.osism.tech` are deprecated and will be turned of on October 20th, 2023.

## Deprecations

### Deprecations via OSISM

For these please also refer to the [upstream deprecation notices](https://release.osism.tech/notes/6.0.0.html#deprecations).

* It is again noted that the old scripts of the form ``osism-`` will be removed in the future.
  A note has been added to the scripts showing this when they are executed.

* The following services are deprecated and will be removed with R6 (OSISM 6.1.0):
  * Patchman
  * Adminer
  * Patchman Client
  * Virtualbmc
  * Bird

## Security Fixes

Throughout the Release 5 development cycle, the SCS project issued two security advisories for upstream components:

* In April 2023 an advisory in Open vSwitch (OvS) ([CVE-2023-1668](https://cve.report/CVE-2023-1668) was issued.
Our [advisory](https://scs.community/security/2023/04/21/cve-2023-1668/).

* In May 2023 an advisory affecting the OpenStack component Cinder ([CVE-2023-2088](https://cve.report/CVE-2023-2088)) was issued.
Our [advisory](https://scs.community/security/2023/05/10/cve-2023-2088/).

## Resolved Issues
Numerous minor issue have been resolved. The most important steps on the IaaS side probably being the move to ceph Quincy
to avoid running out of upstream support. On the container side, the fix of storage snapshots is probably most significant.

For details, we again refer to the [OSISM](https://release.osism.tech/notes/6.0.0.html) and
[k8s-cluster-api-provider](https://github.com/SovereignCloudStack/k8s-cluster-api-provider/blob/main/Release-Notes-R5.md) release notes.

## Standards Conformance
A new certification set is expected in December. It will ensure we
run all automated tests also for all new standards, such as
[v3 flavor naming](https://github.com/SovereignCloudStack/standards/blob/main/Standards/scs-0100-v3-flavor-naming.md),
and the (previously included) [v1 standard flavors](https://github.com/SovereignCloudStack/standards/blob/main/Standards/scs-0103-v1-standard-flavors.md) -- which includes the [new SSD flavors](https://github.com/SovereignCloudStack/standards/blob/main/Standards/scs-0110-v1-ssd-flavors.md$a), the [v1 entropy standard](https://github.com/SovereignCloudStack/standards/blob/main/Standards/scs-0101-v1-entropy.md). We have also split image naming and standard image recommendations into [v1 standards images](https://github.com/SovereignCloudStack/standards/blob/main/Standards/scs-0104-v1-standard-images.md).

Requirements for [k8s version recency](https://github.com/SovereignCloudStack/standards/blob/main/Standards/scs-0210-v1-k8s-new-version-policy.md), [default storage class](https://github.com/SovereignCloudStack/standards/blob/main/Standards/scs-0211-v1-kaas-default-storage-class.md) as well as requirements to the [container registry](https://github.com/SovereignCloudStack/standards/blob/main/Standards/scs-0212-v1-requirements-for-container-registry.md) have been captured.

The IAM area has seen ADRs on the chosen architecture.

The (design) decisions on the metering work as well as on the status page project have also been
captured.

The standards and the standards compliance of our operators' clouds can be seen in the
[standards section of our doc pages](https://docs.scs.community/standards) while the raw content is developed
and discussed in the respective [github standards repository](https://github.com/SovereignCloudStack/standards).

The SCS reference implementation follows all approved SCS standards.

## Release Tagging
Relevant repositories have been tagged with `v6.0.0` tag.
For some repositories `maintained/v6.x` and `maintained/v6.0.x` branches have been created.

## List of known issues & restrictions in R5
Nothing that we are aware of at this point.

## Contributing

We appreciate contribution to strategy and implementation, please join
our community -- or just leave input on the github issues and PRs.
Have a look at our [How to contribute page](https://scs.community/contribute/).

## Thanks

Our wonderful community of integrators, operators, contractors and volunteers
made R5 possible. The project management team is employed by the OSB Alliance
and we as well as the contractors are paid thanks to funding from the German
Ministry for economic affairs and climate action. We build on top of a lot of
existing open source code from the CNCF, the OIF and various others and we
try to contribute back as much as we can.
