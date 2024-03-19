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

### IAM

* Updated Keycloak image to 23.0.6.
* [Custom SCS-Keycloak image](https://registry.scs.community/harbor/projects/22/repositories/scs-keycloak/artifacts-tab) now includes the [Home-IdP-discovery](https://github.com/sventorben/keycloak-home-idp-discovery) plugin.

## New Features (Highlights)

### Operator focused improvements

A kubernetes engine, via [k3s](https://k3s.io), has been introduced to the control plane of the IaaS reference
implementation.

`osism` now deploys Keycloak to k3s via [codecentric/keycloakx](https://github.com/codecentric/helm-charts/blob/master/charts/keycloakx/README.md) helm chart and [CloudNativePG](https://github.com/cloudnative-pg/charts) operator.

Rotation of the Octavia Amphora images has been added to the `osism` command. `osism manage image octavia` will rotate
the image, which is rebuilt on a daily basis.

#### OpenStack Health Monitor

A new monitoring stack will replace the old
[openstack-health-monitor](https://github.com/SovereignCloudStack/openstack-health-monitor/).
Nevertheless, it is currently still in heavy use and has thus seen a few improvements
responding to challenges observed in real-life clouds:
* A new [installation guide](https://docs.scs.community/docs/operating-scs/guides/openstack-health-monitor/Debian12-Install)
* Robustness against leaking volumes even with overloaded cinder API
* Robustness against leaking ports from OVN LB health-monitor
* Robustness against leftover keypairs
* Avoid some followup errors when VM creation failed
* Add logic to startup os-health-mon in tmux windows via `systemctl --user`

### SCS Developer focused improvements (testbed and k8s cluster management)

#### KaaS

* Every component of Cluster Stacks brings a Tilt environment for local test and development
* With csctl Cluster Stacks assets can be created and tested locally without uploading to GitHub

### Preview: Domain Manager Persona

A Domain Manager role has been established in a [standard draft in SCS](https://docs.scs.community/standards/scs-0302-v1-domain-manager-role) aiming to allow self-service capabilities for customers at the identity API.
Work is in progress to [contribute this functionality to OpenStack Keystone](https://bugs.launchpad.net/keystone/+bug/2045974) and the corresponding [upstream spec](https://review.opendev.org/c/openstack/keystone-specs/+/903172) is currently under review.
The feature is expected to be available in the next SCS release.

### Preview: Central API

To improve the experience of SCS cloud customers, the idea of a "Central API" was discussed. Such API should enable customers to manage various "as-a-Service" resources. For example: OpenStack instances as well as Kubernetes clusters as well as Keycloak OAuth2 clients.
Read about the tradeoffs and ideas in the [central-api repository](https://github.com/SovereignCloudStack/central-api) and feel free to test out the [POC](https://github.com/SovereignCloudStack/central-api/blob/0422e8ca24667b04b7364ffa461f85c3de51a50a/poc-setup.md).

### Preview: Keycloak Home-IdP-discovery

To improve flexibility of onboarding new customer domains via IdP federation, SCS now deploys Keycloak with the [Home-IdP-discovery](https://github.com/sventorben/keycloak-home-idp-discovery) plugin. The idea is, that Keystone federates out to a single Keycloak "proxy" realm (called `osism` in the testbed) and using the plugin, Keycloak can identify the user specific realm from an email-format login-ID. Operators can create dedicated realms for each customer and Keycloak uses internal federation to redirect from the "proxy" realm to the specific customer realm. In the customer ream they can configure IdP federation (OpenID-Connect or SAML) to their own IAM solution. The [IAM section](https://docs.scs.community/docs/iam) of the SCS documentation shall be extended to detail the configuration. SCS is working upstream to contribute required enhancements in the
mapping of users, groups and roles from OpenID-Connect token claims to the OpenStack Keystone access management.


### Upcoming: automated pentesting security pipeline

In the beginning of 2024 we started with the conception and implementation of an automated security pipeline. This pipeline will serve as a security scanning tool that is either triggered by deployments or due to manual activation in your infrastructure. The pipeline will contain six tools: Naabu, httpx, Nuclei, Greenbone (Community Edition), ZAP and DefectDojo. While the first three are meant to discover network-related issues like open ports and attack vectors, GCE and ZAP will deliver deeper security inspections of hosts in your SCS environment. Reports are collected in DefectDojo, allowing an overview over security-related changes and seeing the current state. The pipeline will be available as a platform tool in R7.

## Upgrade/Migration notes

* For the IaaS reference implementation, please refer to the [OSISM 7.0.0 Upgrade Notes](https://release.osism.tech/notes/7.0.0.html#upgrade-notes).

## Removals

* Please check the removals for OSISM in the [upstream removal notices](https://release.osism.tech/notes/7.0.0.html#removals).

## Deprecations

* KaaSv1 is still provided with R6, but we do not intend to include it in R7 again.
  We want to rather focus on the feature completeness of the much more future-proof
  cluster-stacks.

* In upstream OSISM the role for deploying the Tang service (`osism.services.tang`) has been deprecated.
  We would like to encourage active contributions in this area via the deprecation, since this piece of
  code is currently not actively maintained nor -- to our knowledge -- actively used.

## Security Fixes

During the R6 development cycle a few security issues were reported and we issued security
advisories and addressed them via maintenance updates. All of these issues are also fixed
in the upcoming R6 release. These include:
* A [OvS vulnerability with crafted Geneve packets and HW acceleration](https://scs.community/de/security/2024/02/20/cve-2023-3966/)
* A [OVN vulnerability against specific BFD packets](https://scs.community/de/security/2024/03/15/cve-2024-2182/)

Other security topics were covered in our community blog as well:
* [Delving into the Technical Depths of Intel-SA-00950 and AMD Cachewarp Vulnerabilities](https://scs.community/2024/01/03/intel-amd-cpu-vulns/)

### Security assessments for IaaS

We invested in a range of penetration tests of the IaaS layer which resulted in valuable insights in security issues and probable improvements (e.g. applying hardening measures):

* External pentesting of components (scanning, blackbox testing)
* Internal pentesting of components with privileged and unprivileged system users (scanning from inside the cluster)
* Scanning and pentesting the environment from a customer workload machine

## Resolved Issues

### Preview: proper scope filtering for domain list API

A fix to a bug where listing domains via Keystone API would return domains not intended to be visible to the requesting entity was [contributed and merged upstream](https://bugs.launchpad.net/keystone/+bug/2041611).
The fix is expected to be available by the next SCS release.

## Documentation

The [docs page](https://docs.scs.commnutiy/) has come a long way in the last 6 months.
It pulls in a lot more content from the various projects and structures it in a much
more accessible way. Look at the [standards](https://docs.scs.community/standards) pages
there to get an impression.

### Highlighted blog posts

* Oct'23: [Progress: OpenStack API access with OpenID Connect in Sovereign Cloud Stack](https://scs.community/2023/10/19/progress-openstack-cli-with-federation/)
* Nov'23: [Confidential Computing in digital sovereign environments](https://scs.community/tech/2023/11/21/confidential-computing-in-digital-sovereign-environments/)
* Dec'23: [SCS observability and monitoring - An opinionated proposal](https://scs.community/tech/2023/12/06/mvp-monitoring/)
* Dec'23: [Cluster Stacks](https://scs.community/2023/12/23/clusterstacks/)
* Feb'24: [SDN scalability improvements](https://scs.community/2024/02/09/sdn-scalability/)

### IAM

The documentation now contains an [IAM overview document](https://docs.scs.community/docs/iam) which explains current
limitations for the dynamic mapping of user roles and shall be extended to explain configuration options for Operators.

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

* Creating loadbalancers in Cloud-in-a-Box installations fails with the
  error message that the VIP subnet does not exist. [OSISM #890](https://github.com/osism/issues/issues/890)
* When using `--provider ovn` with a loadbalancer health-monitor, we leak ports `ovn-lb-hm-$SUBNETID` in all
  but the VIP subnet, if we clean up the LB members before the health-monitor. This is tracked as
  [OSISM issue #921](https://github.com/osism/issues/issues/921). Deleting the health-monitor before the
  members or using `openstack loadbalancer delete --cascade` avoids this issue.
* With amphora loadbalancers, we can end up in situations that LB deletion does no longer work due to
  a failover or a failed creation of the vrrp port. This is tracked in
  [OSISM issue #925](https://github.com/osism/issues/issues/925). An upstream fix exists and a backport
  is already underway.

We expect to resolve these issues with a maintenance update.

### KaaS

Some features of KaaS v1 are not available yet in KaaS v2 because they are WIP in upstream CAPO.
This includes the creation of some of the optional components such as e.g. the deployment
of ingress service, cert-manager, flux, harbor. More importantly, we do not yet have the
handling of restrictive security groups implemented nor the ability to avoid OpenStack

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

### IAM

Since Keycloak is a Java application it requires importing certificates into its certificate store.
As the Keycloak pod in k3s now runs the service as non-root user, it gets more challenging to import
custom certificates from arbitrary customers for IdP federation. In case this topic is intersting for
specific deployments, SCS would be interesting to hear about it and discuss how to implement this in
a usable way.

## Contributing

We appreciate contribution to strategy and implementation, please join
our community -- or just leave input on the github issues and PRs.
Have a look at our [How to contribute page](https://scs.community/contribute/).

## Thanks

We have had considerable help from many partners and upstream projects during
the R6 development cycle. We continue to be grateful for the generous support
from providers that support with infrastructure that we can use for testing and
development. plusserver has been particularly generous and also helped us
finding a few issues during the pre-release phase by upgrading test
environments and testing them.

