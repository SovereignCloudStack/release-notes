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

### Operations

#### Observability

The [Observability stack reference implementation](https://github.com/SovereignCloudStack/k8s-observability) is based on [dNation Kubernetes monitoring stack v3.5.0](https://github.com/dNationCloud/kubernetes-monitoring-stack)
and delivers the following components:

* Monitoring of [infrastructure services](https://docs.scs.community/docs/operating-scs/components/monitoring/docs/overview) such as Kubernetes clusters, physical or virtual machines, and infrastructure endpoints
* [Zuul CI monitoring](https://docs.scs.community/docs/operating-scs/components/monitoring/docs/zuul)
* [Matrix chat notifications](https://docs.scs.community/docs/operating-scs/components/monitoring/docs/alertmanager)
* [OAUTH authentication](https://docs.scs.community/docs/operating-scs/components/monitoring/docs/oauth)
* [IaaS monitoring](https://docs.scs.community/docs/operating-scs/components/monitoring/docs/iaas) is included as a technical preview

#### Zuul

* Created a development deployment for advancement testing.
* Simplified the installation requirements.
* Updated the Zuul repository's README file to include complete installation instructions for Terraform an Ansible.
* Added the clouds data into the ansible vault.
* Created an independent ansible Zuul role.
* Moved the custom Zuul tasks from the playbook and incorporated them into the role.
* Moved the custom Zuul variables from the playbook into the role's variables.
* Included the installation of Letsencrypt which adds certificates as part of the installation.
* Added a volume on the VM for the MariaDB instance to use for continuity.
* Added volumes on the VM for Zookeeper to use for the data snapshots and datalog.

## New Features (Highlights)

### Operator focused improvements

A kubernetes engine, via [k3s](https://k3s.io), has been introduced to the controlplane of the IaaS reference
implementation.

The new monitoring stack mentioned above replaces the old
[openstack-health-monitor](https://github.com/SovereignCloudStack/openstack-health-monitor/).
Nevertheless, it is currently still in heavy use and has thus seen a few improvements
responding to challenges observed in real-life clouds:
* A new [installation guide](https://docs.scs.community/docs/operating-scs/guides/openstack-health-monitor/Debian12-Install)
* Robustness against leaking volumes even with overloaded cinder API
* Robustness against leaking ports from OVN LB health-monitor
* Robustness against leftover keypairs
* Avoid some followup errors when VM creation failed
* Logic to startup os-health-mon in tmux windows via `systemctl --user`

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
* KaaSv1 is provided with R6 again, but we do not intned to include it in R7 again.
  We want to rather focus on the feature completeness of the much more future proof
  cluster-stacks.

## Security Fixes
During the R6 development cycle a few security issues were reported and we issued seucirty
advisories and addressed them via maintenance updates. All of these issues are also fixed
in the upcoming R6 release. These include:
* A [OvS vulnerability with crafted Geneve packets and HW acceleration](https://scs.community/de/security/2024/02/20/cve-2023-3966/)
* A [OVN vulnerability against specific BFD packets](https://scs.community/de/security/2024/03/15/cve-2024-2182/)

## Resolved Issues

### Preview: proper scope filtering for domain list API

A fix to a bug where listing domains via Keystone API would return domains not intended to be visible to the requesting entity was [contributed and merged upstream](https://bugs.launchpad.net/keystone/+bug/2041611).
The fix is expected to be available by the next SCS release.

## Documentation
The [docs page](https://docs.scs.commnutiy/) has come a long way in the last 6 months.
It pulls in a lot more content from the various projects and structures it in a much
more accessible way. Look at the [standards](https://docs.scs.community/standards) pages
there to get an impression.

## Standards Conformance
The standards have evolved, increasing the amount of guarantees that software developers
and operators have for workloads that work across SCS-compatible clouds.
The [SCS-compatible IaaS-v4](https://docs.scs.community/standards/scs-compatible-iaas)
has seen improvements and better test coverage; the OSISM IaaS reference implementation
fulfills all of these standards in the default configuration.

For the Kubernetes layer, we have our first set of standards almost finalized.
Some of the standards for [SCS-compatible KaaS-v1](https://docs.scs.community/standards/scs-compatible-kaas)
are unfortunately hard to test, so are working on adding some more tests before
we cut it in stone. We aim for both KaaSv1 and v2 to fulfill the standards.
(Future standards will likely not be fulfilled by KaaSv1 as it's being deprecated.)

## Release Tagging
A number of repositories follow OSISM's example and use the `7.0.0` or `v7.0.0` tag
to denote SCS Release 6.

## List of known issues & restrictions in R6

### IaaS

#### Loadbalancer service (octavia)
* Creating loadbalancers at least on upgraded Cloud-in-a-Box installations fails with the
  error message that the VIP subnet does not exist. [OSISM #890](https://github.com/osism/issues/issues/890)
* When using `--provider ovn` with a loadbalancer health-monitor, we leak ports `ovn-lb-hm-$SUBNETID` in all
  but the VIP subnet, if we clean up the LB members before the health-monitor. This is tracked as
  [OSISM issue #921](https://github.com/osism/issues/issues/921). Deleting the health-monitor before the
  members or using `openstack loadbalancer delete --cascade` avoids this issue.
* With amphora loadbalancers, we can end up in situations that LB deletion does no longer work due to
  a failover or a failed creation of the vrrp port. This is tracked in
  [OSISM issue #925](https://github.com/osism/issues/issues/925). An upstream fix exists that we expect
  to get backported in the future.

We expect to resolve these issues with a maintenance update.

### KaaS
Some features of KaaS v1 are not available yet in KaaS v2 because they are WIP in upstream CAPO.
This includes the creation of some of the optional components such as e.g. the deployment
of ingress service, cert-manager, flux, harbor. More importantly, we do not yet have the
handling of restrctive security groups implemented nor the ability to avoid OpenStack
scheduling more than one control plane node on the same host (hypervisor).

For this reason, we are including KaaS v1 (k8s-cluster-api-provider) in the R6 release,
so existing users can upgrade to the latest upstream code and continuing using it as
a maintained solution while they evaluate the migration to KaaS v2 (cluster-stacks).

We will address most of the gaps during the next release cycle.

KaaS v1 should not be used for new deployments; we intend to remove it with the next
release (R7).

## Contributing

We appreciate contribution to strategy and implementation, please join
our community -- or just leave input on the github issues and PRs.
Have a look at our [How to contribute page](https://scs.community/contribute/).

## Thanks

We have had considerable help from many partners and upstream projects during
the R6 development cycle. We continue to be grateful for the generous support
from providers that support with infrastructure that we can use for testing and
development. PlusServer has been particularily generous and also helped us
finding a few issues during the pre-release phase by upgrading test
environments and testing them.

