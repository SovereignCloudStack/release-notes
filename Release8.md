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
