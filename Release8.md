# Release Notes for SCS Release 8

This document is work in progress for the upcoming Release 8.
Release 8 will be released in March 2025.
This note will be removed, once Release 8 is released and these notes are valid.

## Scope

## Component Versions and User-visible improvements (highlights)

- Cluster API Provider OpenStack v0.12
  - adds OpenStack Resource Controller (ORC) as external component

## New Features (Highlights)

### KaaS

- Cluster Stacks `SCS` to use Multi Stage Addons
- ORC Image kind in Cluster Stacks `SCS`
- Helm charts to manage Cluster Stack Operator
  - simplifies configuration of release source
  - additional RBAC for provider specific resources can be configured

### Operator focused improvements

### SCS Developer focused improvements (testbed and k8s cluster management)

## Upgrade/Migration notes

- The container registry has been upgraded to Harbor v2.12.2
  - Since the last SCS release, the container registry has introduced numerous features and enhancements. Key highlights include:
    - Improved granular access control for robot accounts, enhancing security and automation
    - Audit logging now includes robot account activities for better traceability
    - Introduced proxy cache bandwidth limits to control network speed when pulling artifacts
    - Optimized LDAP onboarding for faster user authentication
    - Support for generating Software Bill of Materials (SBOM) of artifacts either manually or automatically
    - Support for OCI Distribution Spec v1.1.0 and more
  - The backup and restore procedure, as documented in the [SCS documentation](https://docs.scs.community/docs/container/components/container-registry/docs/backup_and_restore), was successfully tested with production data during the migration from the SCS1 to the SCS2 environment
- The observability platform has been upgraded to dN Kubernetes Monitoring Stack v3.8.1
  - This upgrade introduces several updates to key observability stack components, including Prometheus Operator v0.74, Thanos v0.37, and Loki v3.2
  - Support for deploying the Prometheus OpenStack Exporter, enabling IaaS layer monitoring within the Observability platform
  - The deployment procedure, as outlined in the [SCS documentation](https://docs.scs.community/docs/operating-scs/components/monitoring/docs/scs-deployment), was successfully tested during the migration to the SCS2 environment. The platform now offers visibility into the new SCS environment

The container registry and observability platform are fundamental and reliable components of the Sovereign Cloud Stack. By actively using them ourselves, we validate their functionality and ensure they are well-suited for seamless adoption by Cloud Service Providers.

## Removals

### KaaS

- Node Image from Cluster Stacks `SCS`
  - keeps specific provider configuration on ClusterAPI level

## Deprecations

### KaaS

- Cluster Stack Provider OpenStack
  - in favor of ORC

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
