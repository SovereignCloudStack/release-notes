# Release Notes for SCS Release 8

The SCS Software (reference implementation) is released in version 8 on 2025-04-09.

## Scope

SCS continues to deliver a coordinated release covering the various modules that comprise
the SCS reference implementation. SCS includes the latest proven upstream technologies and
ensures that upgrades to R8 are smooth using the CI processes. The reference implementation
with the default settings fulfills the requirements from SCS-compatible IaaS v5.1 and
KaaS v1.0 standards.

## Component Versions and User-visible improvements (highlights)

- Cluster API Provider OpenStack v0.12
  - adds OpenStack Resource Controller (ORC) as external component
- OpenStack 2024.2 is included in R8, but can be skipped in case Operators
  want to make use of skipping over it and upgrade directly from 2024.1 to 2025.1
  (with the next release of SCS).

### Cluster Stacks

The SCS Cluster Stacks for R8 can be found on the SCS Harbor at
registry.scs.community/kaas/cluster-stacks with the tags

- `openstack-scs-1-30-v3`
- `openstack-scs-1-31-v2`
- `openstack-scs-1-32-v1`

## New Features (Highlights)

### IaaS

- Support of OpenStack 2024.1 and OpenStack 2024.2 (the SLURP model will be supported in future,
  OpenStack 2025.1 (Epoxy) is the first SLURP release, you can upgrade to 2025.1 from 2024.2 (Dalmatian),
  but (new) also directly from 2024.1 (Caracal) if you prefer to skip the .2 release.
    - The domain-manager enhancements from the SCS community have been included in the upstream
      2024.2 OpenStack release.
- Support of Ceph Quincy and Ceph Reef
- Support of Kubernetes 1.31 for the Kubernetes cluster that is integrated in the IaaS layer for extensions
- The Netbox integration has been completely revised, there is now a dedicated tool called
  Netbox Manager that allows you to manage content from the Netbox with an independent repository
- There is now a special burn-in image for preparing new hardware (this is used when new cloudpods
  are created and Ironic cannot yet be used)
- A bunch of new services such as Wazuh agent, Teleport agent or Dnsmasq
- Enhancement of the testbed, in specific with Ironic to improve the CI capabilities

### KaaS

- Cluster Stacks `SCS` to use Multi Stage Addons
  - Support for Multi Stage Addons, a Cluster Stack Operator feature that enables structured addon upgrades, was introduced in R7 as part of KaaS.
    In R8, this feature has been fully implemented into the stable SCS Cluster Stacks for OpenStack.
    Now, key addons, including the Container Network Interface (CNI), Container Storage Interface (CSI), and Cloud Controller Manager (CCM), are installed in a predefined sequence after the Kubernetes control plane is initialized.
    The upgrade order of these addons is also orchestrated before the Kubernetes cluster version is upgraded. This ensures smooth upgrades and stability throughout the Kubernetes version transitions.
- ORC Image kind in Cluster Stacks `SCS`
  - Cluster Stacks `SCS` can now include an ORC Image resource, which simplifies and automates the upload of cloud images to OpenStack
  - This functionality was previously handled by the Cluster Stack Provider for OpenStack, which is now deprecated in favor of ORC. ORC offers enhanced support, including checksum verification for improved reliability
- Helm charts to manage Cluster Stack Operator
  - simplifies configuration of release source
  - additional RBAC for provider specific resources can be configured
- Cluster Stacks with latest Kubernetes Cluster API and latest Kubernetes versions (1.32)

### Operator focused improvements

- The openstack-health-monitor has seen minor improvements:
  - The calculation of the percentage of IOs that have latencies above 10ms has been corrected.
  - It can monitor the availability of endpoints for heat, swift, manila, octavia, barbican, senlin, magnum, aodh, gnocchi and ironic
    just doing simple list (GET) calls if these are available and the client tooling is present (option -X).
  - The plan is still to replace openstack-health-monitor with the scs-health-monitor in the future.
- The container registry (SCS release v8.0.0) has been upgraded to Harbor v2.12.2
  - Since the last SCS release, the container registry has introduced numerous features and enhancements. Key highlights include:
    - Improved granular access control for robot accounts, enhancing security and automation
    - Audit logging now includes robot account activities for better traceability
    - Introduced proxy cache bandwidth limits to control network speed when pulling artifacts
    - Optimized LDAP onboarding for faster user authentication
    - Support for generating Software Bill of Materials (SBOM) of artifacts either manually or automatically
    - Support for OCI Distribution Spec v1.1.0 and more
  - The backup and restore procedure, as documented in the [SCS documentation](https://docs.scs.community/docs/container/components/container-registry/docs/backup_and_restore), was successfully tested with production data during the migration from the SCS1 to the SCS2 environment
- The observability platform (SCS release v8.0.0) has been upgraded to dNation Kubernetes Monitoring Stack v3.8.1
  - This upgrade introduces several updates to key observability stack components, including Prometheus Operator v0.74, Thanos v0.37, and Loki v3.2
  - Support for deploying the Prometheus OpenStack Exporter, enabling IaaS layer monitoring within the Observability platform
  - The deployment procedure, as outlined in the [SCS documentation](https://docs.scs.community/docs/operating-scs/components/monitoring/docs/scs-deployment), was successfully tested during the migration to the SCS2 environment. The platform now offers visibility into the new SCS environment

The container registry and observability platform are fundamental and reliable
components of the Sovereign Cloud Stack. By actively using them ourselves, we
validate their functionality and ensure they are well-suited for seamless
adoption by Cloud Service Providers.

## Upgrade/Migration notes

- Upgrading SCS KaaS R7 to R8 for all OpenStack installations requires installing the ORC component into the management cluster. ORC was split off from the Cluster-API Provider OpenStack v0.12
  - [ORC installation]((https://github.com/k-orc/openstack-resource-controller?tab=readme-ov-file#installation) should be performed as part of the upgrade to Cluster-API Provider OpenStack v0.12
  - Also the CSO needs permissions to manage ORC images. This can be applied with CSO Helm values, see [SCS Cluster Stack](https://github.com/SovereignCloudStack/cluster-stacks/blob/main/providers/openstack/scs/README.md) for an example.

## Removals

### IaaS

- virtualbmc service (in favor of Redfish with sushy)
- keycloak and cloudnative_pg service
- tang and clevis service
- SCS metering service
- OpenStack health monitor service

### KaaS

- Node Image from Cluster Stacks `SCS`
  - keeps specific provider configuration on ClusterAPI level

## Deprecations

### Ops Tooling

- openstack-health-monitor will be removed in the future in favor of the scs-health-monitor

### KaaS

- Cluster Stack Provider OpenStack
  - in favor of ORC

## Standards Conformance

The SCS reference implementation fulfills the relevant SCS standards.

In particular, the IaaS layer from OSISM fulfills the OpenStack Interop Guidelines 2022.11
and the SCS-compatible IaaS standards v5.1.

The KaaS layer with the SCS Cluster Stacks
fulfills the SCS-compatible KaaS standards v1 and pass the CNCF e2e tests. When chosing the
default cilium CNI provider, we observe two failed test cases for network policies.
These failures are not specifc to SCS cluster stacks and we are accompagnying the
upstream evolution around them. We expect to provide solutions, workarounds, or waivers
for these in the future but do not see particular relevance for these.

## Release Tagging

OSISM uses the tag 9.0.0 for SCS R8 and some of the related repositories in the SCS project
use a similar tag. Some repositories use maintained (stable) branches where the amount of changes
is limited to mainly bug and security fixes to make life easier for our operators that need to
stay on top of our security fixes without incurring too much change. Please note that by default,
we will stop maintaining the old R7 branches after May.

## List of known issues & restrictions in R8

Please read above notes on ORC installation and the CNCF e2e network policy test failures.

The limitation in capo that does not allow us to ensure that control plane nodes of a k8s cluster
never end up on the same host continues to exist; the k8s community continues to insist on a
notion of failure domains that does neither exactly correspond to IaaS availability zone nor
to hosts. We are arguing for a pragmatic approach here and still hope to be able find a solution
that avoids our own downstream divergence.

Progress has been made on the OVN load balancer file descriptor leak; yet it is not completely
resolved yet. Nevertheless, we consider the OVN load balancer the tool of choice for most
scenarios where L7 (https) loadbalancing is *not* required, such as e.g. when used in front
of the kubernetes API or a k8s ingress/gateway. Please see the R7 release notes for more details.

## Documentation

As before, the documentation has been kept up-to-date with changes in the implementation.

## Contributing

We appreciate contribution to strategy and implementation, please join
our community -- or just leave input on the github issues and PRs.
Have a look at our [How to contribute page](https://docs.scs.community/community/).

## Next releases

We intend to continue doing coordinated releases. However, OSISM has stated an intention
to publish an OSISM release containing OpenStack 2025.1 ahead of the normal Sep/Oct time
that our partners have come to expect, so there may be a one-off. As soon as the plans
solidify, we'll announce it via our matrix channels and mailing lists.

## Thanks

Numerous community members and companies have contributed to this release. Notably the
elected members of the project board have stepped up and taken responsibility to drive
the efforts to develop the software. We'd like to call out dNation, OSISM, UhuruTec,
syself, syseleven and S7n Cloud Services for their contributions. We'd also like to
thank ScaleUp Technologies, plusserver, Wavecon, Artcodix, and Cleura for sponsoring
infrastructure. We also rely on the standardization work that is governed by the Forum
SCS-Standards in the OSBA and funded by 17 supporting companies and supported by
the ALASCA association. SCS has received funding from the German Government (BMWK)
between 2021 and 2024 in the context of supporting Gaia-X.
