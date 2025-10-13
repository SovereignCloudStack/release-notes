# Release Notes for SCS Release 9

Release 9 was [released on 2025-09-29](https://www.sovereigncloudstack.org/announcements/release9/).

## Scope

The main focus of the Release 9 of the SCS reference implementations
was to stay up-to-date with current software, refactoring code for
easier maintainability and improved hardware management

## Component Versions and User-visible improvements (highlights)

### IaaS
[OSISM-10](TODO: LINK) comes with [OpenStack](https://www.openstack.org/) 2025.1
([Epoxy](https://releases.openstack.org/epoxy/)) and 
[Ceph Reef](https://docs.ceph.com/en/latest/releases/reef/). The host images
now use Ubuntu 24.04 LTS by default everywhere.

### KaaS
The node images are also built with Ubuntu 24.04 LTS now. The new cluster stack ["scs2"](https://github.com/SovereignCloudStack/cluster-stacks/tree/main/providers/openstack/scs2)
starts with [Kubernetes 1.33.x](https://kubernetes.io/releases/)
and leverages Cluster API v1.10 and Cluster API Provider OpenStack v0.12
as well as the matching Cilium, Cinder CSI and OCCM versions.

## New Features (Highlights)

### Operator focused improvements
The new [netbox-manager](https://github.com/osism/netbox-manager) supports
a more complete control of the environment
using the inventory from netbox. There are enhanced capabilities to generate
configuration files from the netbox data including networking gear, such as
switches. To keep an overview, there are now web-interfaces for events and
status.

~...TODO...~

The [OpenStack Health Monitor](https://github.com/SovereignCloudStack/openstack-health-monitor/)
has received a maintenance fix to support the
latest openstackclient v7.x.

### SCS Developer focused improvements
The OpenStack 2024.2 (Dalamatian) release brought the upstreamed
[Domain Manager role](https://specs.openstack.org/openstack//keystone-specs/specs/keystone/2024.1/domain-manager-persona.html);
this allows for self-service management of users and projects and was a
downstream configuration in SCS before. Epoxy brings further improvements for
handling PCI-passthrough devices and can now support live-migration in more
setups despite VMs with PCI-passthrough access; this is relevant for GPUs
that are used for AI acceleration.

~...TODO... OSISM testbed, CiaB~

The new `scs2` cluster stack which succeeds the `scs` cluster stack
has streamlined configuration.
Rather than generating the secrets in two different formats (which was done
using a [helper helm chart](https://github.com/SovereignCloudStack/openstack-csp-helper)
previously), we now use only one secret, simplifying
the handling by only requiring one ClusterResourceSet to be managed.
This also allows to support self-signed certificates (a custom CA for
the OpenStack API) without trouble.

The [ClusterClass variables](https://github.com/SovereignCloudStack/cluster-stacks/blob/f5f9a4260d32bac33ff87146cb1f88ac55288bc9/providers/openstack/scs2/cluster-class/templates/cluster-class.yaml#L35)
have been cleaned up and now follow a more
consistent camelCase naming scheme. The defaults now use diskless flavors,
providing a better preconfiguration on SCS virtualization infrastructure.
Load balancers, flavors, disks etc. can be configured using ClusterClass
variables. The ClusterClass releases no longer bundle the Helm charts
for the cluster addons (such as CNI, CSI, CCM, metrics). Instead, they
are retrieved alongside the container images during cluster setup time
by the Cluster Stack Operator (CSO). A new version v0.2.0-alpha.1 of
CSO has been released to support this -- it also still supports the old
way with bundled helm charts. In either case, the version is pinned by
the specific ClusterClass, so users remain on validated paths.

The [node images](https://swift.services.a.regiocloud.tech/swift/v1/AUTH_b182637428444b9aa302bb8d5a5a418c/openstack-k8s-capi-images/)
are now built using Ubuntu 24.04 LTS by default.

## Upgrade/Migration notes
SCS has always supported the seamless upgrade from the previous
version and has invested significant effort to validate this in order
to avoid issues that would cause SCS operators to remain behind.
With OpenStack 2025.1 in OSISM-10, it's possible to do a single-step
upgrade from OpenStack 2024.1 (OSISM-8 and 9), leveraging the work
that the upstream OpenStack community does to support the so-called
Skip-Level-Upgrade-Release-Process ([SLURP](https://docs.openstack.org/project-team-guide/release-cadence-adjustment.html)).

The new cluster stack "scs2" can live next to existing "scs"
cluster stacks. The required update to the new CSO (v0.2.0-alpha.1)
does support both. The [script collection](https://github.com/SovereignCloudStack/scs-training-kaas-scripts/)
(that was developed for the international SCS trainings) also supports
both.

There is currently no automated upgrade from "old" scs clusters to
new scs2 clusters; as there are 1:1 relationships between old and
new cluster variables and a possibility to convert secrets this is
possible. The code to do this conversion automatically is still
in development and will be carefully tested before being released.

The scs2 clusters support Kubernetes 1.33.x and future K8s versions.
At release time, the cluster stack `openstack-scs2-1-33-v1` containing
k8s-v1.33.4 was current.
The old `scs` cluster stack will end with the final 1.31.x and 1.32.x patch
levels.

## Resolved Issues
Supporting custom CAs with Cluster Stacks was painful before and
is straightforward with the scs2 cluster stacks now.

## Standards Conformance
The new image metadata property `os_purpose` has been implemented
in the [OpenStack image manager](https://github.com/osism/openstack-image-manager); this allows a future
[standards](https://docs.scs.community/standards/scs-0102-v1-image-metadata) 
version where image names for the [mandatory images](https://docs.scs.community/standards/scs-0104-v1-standard-images)
no longer need
to be prescribed to be uniquely identifiable.

The [OpenStack flavor manager](https://github.com/osism/openstack-flavor-manager)
now has a parameter that allows to
limit recommended flavors to only be created up to a certain amount
of memory. This allows recommending larger flavors without them to
be created automatically in clouds that have relatively small
compute hosts.

A default installation of OSISM passes the v5.1
[SCS-compatible IaaS tests](https://docs.scs.community/standards/scs-compatible-iaas).
It should be noted that the OpenStack-powered Compute tests
have become harder to perform as the upstream RefStack project is
no longer maintained. The community is working on documentation
and/or code to provide guidance to operators how to perform the tests.

The k8s clusters created with the scs and scs2 cluster stacks
pass the [CNCF e2e conformance tests](https://github.com/cncf/k8s-conformance),
with the exception of the `HostPort validates that there is no conflict
between pods with same hostPort but different hostIP and protocol`
test, which is not supported with the our default cilium CNI.
This test tests implementation details that are not portable and
we have thus decided to officially ignore it in the conformance
assessment.

## Release Tagging
~TODO: OSISM ...~

The new cluster stacks "scs2" are tagged `openstack-scs2-1-33-v1`
in the [SCS registry](https://registry.scs.community/), the source
repository carries the same tag. It requires at least version (and
tag) `v0.2.0-alpha.1` of the cluster-stack-operator (or later) to
work.

## List of known issues & restrictions in R9
~TODO: OSISM 10~

As mentioned above, the migration script and docs are still in
development for conversion of `scs` to `scs2` cluster stacks.

Due to the way that the Helm chart of the OCCM currently creates
the `cloud.conf` configuration file, it is not straightforward
to control the type of load balancer that OCCM creates for the
cluster upon demand. So, currently, the cloud's default provider
will be used. We wanted to add a ClusterClass variable that
allows to override this and explicitly configure `octavia-ovn`
or `octavia-amphora`, but the best way to achieve this is still
under discussion. This will be delivered with a future update
and will not be a breaking change.
 
Currently, when creating clusters, the successful start of CoreDNS
and many other pods depends on internal name resolution, which
only succeeds after the OCCM has started successfully. As OCCM
is scheduled on the worker nodes, this slows down the availability
of the cluster and we currently require more than 5 minutes for
the cluster to become available. We believe this can be optimized
and will look into it.

## Contributing
We appreciate contribution to strategy and implementation, please join
our community -- or just leave input on the github issues and PRs.
Have a look at our [How to contribute page](https://docs.scs.community/community/).

## Thanks
The release was only possible due to the project board's members
enagement and the community contributing code. We are lucky to
be part of an active community that strives to create and
release high-quality software.
