# Release Notes for SCS Release 4

This document is work in progress for the upcoming Release 4.
Release 4 will be released on March 22nd, 2023. 
This note will be removed, once Release 4 is released and these notes are valid.


## Scope

Release 4 has been developed alongside a set of associated outcomes. These outcomes are comprised of:

* SCS is standardized
* SCS is federated
* SCS is continuously built and tested
* SCS is understandable
* SCS enables Operators with an excellent toolbox

The SCS project is completly developed in the open, based on the principles of the four opens. Due to this a lot of our work can be tracked and used continously without waiting for the half-year releases. Especially, but not limited to, this includes our efforts in regards to documentation and our standards.

One of the major highlights that happened in the R4 development cycle is our work on assuring _SCS is understandable_.
Be sure to look at [our new documentation entry point](https://docs.scs.community).
We have created a [systematic approach](https://github.com/SovereignCloudStack/docs/blob/main/community/contribute/adding-docs-guide.md) to structure documentation which already has been implemented for the [OpenStack Image Manager](https://docs.scs.community/docs/category/openstack-image-manager/),
the [OSISM testbed](https://docs.scs.community/docs/category/osism-testbed/) and the [K8s Cluster API Provider](https://docs.scs.community/docs/category/k8s-cluster-api-provider/). More will follow in a continuous manner.

Our community has creates a growing amount of [blog articles](https://scs.community/blog/) which also help to understand the SCS project, its community and the technology that is worked on.

## Component Versions and User-visible improvements (highlights)

* [OpenStack Zed release](https://releases.openstack.org/zed/highlights.html)
* Ceph Quincy is available, the default release of Ceph is still Pacific.
* The base infrastructure is provided by
  [OSISM 5.0.0](https://release.osism.tech/notes/5.0.0.html)
  which in turn builds on top of kolla and kolla-ansible.
* With [Cloud-in-a-Box](https://github.com/osism/cloud-in-a-box) there is an easy way to get SCS up and running on a single hardware node as a test environment. There are two blog posts ([part 1](https://scs.community/2023/03/15/ciab/) and [part2](https://scs.community/2023/03/15/ciab-2/)) covering it.
* For new deployments of the IaaS reference implementation Ubuntu 22.04 is recommended while existing installations can be upgraded to R4 while staying on Ubuntu 20.04. With Release 5, upgrading to Ubuntu 22.04 will be required.
* With [osism/node-image](https://github.com/osism/node-image) an iso image for much easier bootstrapping of new OSISM environments is available now
* The software for our [Kubernetes Cluster-API reference implementation](https://github.com/SovereignCloudStack/k8s-cluster-api-provider) has been updated and highlights are covered in own [release notes](https://github.com/SovereignCloudStack/k8s-cluster-api-provider/blob/main/Release-Notes-R4.md).<!--FIXME: We should link to docs.scs.community here.-->


## New Features (Highlights)

### Operator focused improvements

* The [Openstack Image Manager](https://github.com/osism/openstack-image-manager) has seen many improvements and is the reference command to assure the images available comply with the [SCS Image Standard]( ) // FIXME LINK
* For Ceph, special playbooks were added to validate the deployment status of the OSD, MON and MGR services in OSISM. The commands for use are `osism validate ceph-osds`, `osism validate ceph-mons`, and `osism validate ceph-mgrs`.
* OVN has been updated to version 22.09.
* OVS has been updated to version 3.0.1.
* The [testbed](https://github.com/osism/testbed) uses per default a proxy for container pulling. This will allow for airgapped installations out of the box. Please note: a full airgap support (with local mirrors, etc.) will follow in a future release.
* The efforts to create a well-maintained status page with well-defined interfaces resulted in an OpenAPI specification (within its own [repository](https://github.com/SovereignCloudStack/status-page-openapi)) which is intended to be implementable by multiple implementations.
* The dashboard of the [OpenStack Health Monitor](https://github.com/SovereignCloudStack/openstack-health-monitor/) is in use by the SCS operators and has proven helpful a number of times in detecting and addressing issues. That said, it only received a few fixes and minor enhancements, as we plan to replace it with a more generic and more maintainable solution soon.
* The k8s clusters built with our k8s-capi implementation now allow controlling the versions of more components; the latest tested and stable versions are used by default (if enabled). The latest version for the cilium CNI for example allows testing the upcoming k8s gateway API.
* The k8s cluster now allows filtering access to the kubernetes API by IP ranges.
* The k8s clusters now have the proxy protocol enabled with the nginx-ingress controller, so client IPs are visible; the previous issue that blocked internal access could be worked around.

### SCS Developer focused improvements (testbed and k8s cluster management)

* The testbed has been significantly simplified for new operators and developers and a [Quick Start](https://docs.osism.tech/testbed/quickstart.html) guide has been added.

## Upgrade/Migration notes

* For the IaaS reference implementation, please refer to the [OSISM 5.0.0 Upgrade Notes](https://release.osism.tech/notes/5.0.0.html#upgrade-notes).
* The k8s Cluster Management solution has an enhanced [upgrade guide](https://docs.scs.community/docs/k8s-cluster-api-provider/doc/Upgrade-Guide) that covers the upgrade of clusters as well as the upgrade of the cluster management server.

## Removals

* The ospurge wrapper script has been removed from the osism.services.openstackclient role. The ospurge project is no longer compatible with the current OpenStack SDK. The command openstack project purge can be used as an alternative.
* The docker-compose package is uninstalled by the osism.commons.docker_compose role. The Compose v2 plugin for Docker is now used instead of the old standalone docker-compose CLI. A dummy script has been added to /usr/local/bin which displays a corresponding message when using docker-compose.
* Further removals from the IaaS reference implementation, please refer to the [OSISM 5.0.0 Removals Section](https://release.osism.tech/notes/5.0.0.html#removals).
* The k8s cluster parameter `ETCD_PRIO_BOOST` that was already unused has been removed as announced with R3.

## Deprecations

### Deprecations via OSISM

For these please also refer to the [upstream deprecation notices](https://release.osism.tech/notes/5.0.0.html#deprecations)

* The role osism.services.bird is deprecated. In future, FRRouting (osism.services.frr) will be used.
* The role osism.services.minikube is deprecated. In future osism.services.k8s will be used.
* Heat is deprecated in favor of more generic Infrastructure as Code tools like Terraform as of now and will be removed in the future (exact removal date is not yet known)
* Swift (currently available as Technical Preview) will be removed in favor of Ceph RGW
* Trove (currently available as Technical Preview) will be removed in favor of Kubernetes database operators
* Skydive (currently available as Technical Preview) will be removed in the future, the project is not maintained anymore, last commit is 8th Jan 2022 (<https://review.opendev.org/c/openstack/kolla/+/869191>)
* The login to a registry with the `osism.services.docker` role is deprecated in favor of the new `osism.commons.docker_login` role.

## Security Fixes

Throughout the Release 4 development cycle, the SCS project issued two security advisories for upstream components:

* In November 2022 an advisory regarding [CVE-2022-3602 and CVE-2022-3786](https://www.openssl.org/news/secadv/20221101.txt) in OpenSSL was issued.
Our [advisory](https://scs.community/security/2022/11/01/advisory-spookyssl/).

* In February 2023 an advisory regarding [CVE 2022-47951](https://cve.report/CVE-2022-47951) in OpenStack components nova and glance was published.
Our [advisory](https://scs.community/security/2023/01/24/cve-2022-47951/).

Fixes were delivered via maintenance updates to existing R3 deployments, but of course also included in the main development branch that became R4.

## Resolved Issues

* Breakage with old kustomize syntax has been addressed.([k8s-capi/#328](https://github.com/SovereignCloudStack/k8s-cluster-api-provider/issues/328))
* The move of k8s container images from k8s.gcr.io to registry.k8s.io needed adjustments.([k8s-capi/#321](https://github.com/SovereignCloudStack/k8s-cluster-api-provider/issues/321))

## Standards Conformance

The last months saw intense work in the standardization area. The process how standards are created has been documented.
The standards are collected in its own [standards](https://github.com/SovereignCloudStack/standards) repository.
A machine readable file lists the required (and optional) standards that apply to "SCS-compatible" conformance at
the IaaS and the Container (KaaS) layer. The referenced executables are used by the compliance checking framework
to test existing implementations for compliance. To run the checker, the tester needs access to the infrastructure
under test (normal user privileges are sufficient) and standard openstack and kubernetes client tools -- or just
use the docker container that is provided.

The public clouds based on the SCS reference implementation from PlusServer and Noris/Wavecon are tested automatically
from us and the live result is visible in [standards page](https://github.com/SovereignCloudStack/standards).
We will enhance the standardization and test coverage significantly in the next months and we hope to list a number
of more clouds there soon.

## Release Tagging

The code is OSISM and a number of SCS repositories will receive the `v5.0.0` tag; some repositories use
`maintained/v5.0.x` and `maintained/v5.x` branches for providing code that only gets bug- and security fixes (5.0.x)
or only those plus minor, backwards-compatible enhancements (5.x).

## List of known issues & restrictions in R4

* The k8s cluster-API code does not work well with OpenStack API endpoints that require trusting a custom CA.

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
