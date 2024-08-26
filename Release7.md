# Release Notes for SCS Release 7

This document is work in progress for the upcoming Release 7.
Release 7 will be published on September 4th 2024.
This note will be removed, once Release 7 is released and these notes are valid.

## Scope

## Component Versions and User-visible improvements (highlights)

### IaaS

The IaaS reference implementation is based on [OSISM 8.0.0](https://osism.tech/docs/release-notes/osism-8)
and delivers the following components:

* [OpenStack 2024.1 (Caracal)](https://releases.openstack.org/caracal/highlights.html)
* Default Ceph version is [Ceph Quincy](https://docs.ceph.com/en/latest/releases/quincy/#v17-2-7-quincy)
  * [Ceph Reef](https://docs.ceph.com/en/latest/releases/reef/#v18-2-4-reef) is available as an option
* Support for deployments on Ubuntu 24.04, CentOS 9 and Debian 12

Throughout the R7 development cycle numerous, eight in total, minor releases of OSISM have been released.
One of the core ideas of SCS is to move towards a very incremental approach so that keeping infrastructure
up to date is made easy - as such many of the features and improvements that are now part of the Release 7
have been shipped as part of one of the minor releases working towards Release 7.

### KaaS

- Cluster Stack `SCS`
- Cluster Stack releases can be hosted on an OCI registry

## New Features (Highlights)

### Operator focused improvements

### SCS Developer focused improvements (testbed and k8s cluster management)

### Tech Preview: Central API

The idea of a central API for all API services in SCS was further evaluated, conceptualized and a PoC implemented. This implementation is now available as a Tech Preview for CSPs to deploy it at their own site.

Deploying the Central API is done into a dedicated Kubernetes cluster, which will then host the Crossplane operator for connections to backend systems. 

End-customers will then be able to create Kubernetes resources in the `api.scs.community` group, for example a `Cluster` resource to create a new Kubernetes workload cluster.

* Repository: https://github.com/SovereignCloudStack/central-api/
* Documentation of PoC: https://docs.scs.community/docs/operating-scs/components/central-api/poc-setup


### Tech Preview: Automated pentesting for IaaS and KaaS

The security team has worked on a methodology for automated security scans that are specifically put together for SCS. This covers both the infrastructure-as-a-service layer and the Kubernetes container layer. 

For IaaS, a pipeline of tools is executed, with each tool covering different aspects of security weaknesses and the anticipated attack surface of the layer.

For KaaS, the focus currently lies on the Trivy scanner, which can be deployed both as an ad-hoc scan and as a continuously running Operator in Kubernetes. This also includes a custom-made exporter for reports.

Since both layers produce vulnerability reports, which CSPs might want to delegate to their security teams, the deployment of DefectDojo is part of the Tech Preview.

For non-vulnerability reports, an export to S3/Swift was implemented and can be adjusted according to the CSP's needs.

* Documentation (currently for IaaS): https://docs.scs.community/docs/operating-scs/components/automated-pentesting/overview/
* Docs for KaaS: https://github.com/SovereignCloudStack/security-k8s-scan-pipeline/tree/feat/initial-pipeline-setup/docs


### Tech Preview: SCS Health Monitor

As announced in the Release Notes for R6, a new health monitor was in the works during the last months. With R7 we are now at the state of releasing the first Tech Preview of the new SCS Health Monitor. It not only covers the infrastructure-as-a-service layer, but also has workflows for Kubernetes tests.

While the SCSHM took many IaaS-related inspirations from the OSHM, the developer team for the new SCSHM has built an entirely new foundation for behavior-driven testing in SCS. The implementation is based on the Python `behave` framework, using the Gherkin language for test definitions. Using Python allows the tool to then use Python-native API cients for OpenStack and Kubernetes, with room for extension for many other tools in SCS as well.

During the test run, metrics and results are collected and exported via a Prometheus Push Gateway into a Grafana-based dashboard.

* Documentation: https://docs.scs.community/docs/category/scs-health-monitor
* Repository: https://github.com/SovereignCloudStack/scs-health-monitor


### KaaS

- Multi-Stage-Addons
- GUI for generating Cluster objects
- csctl-plugin-openstack
- More Providers
  - Kamaji
  - Metal³
  - KubeVirt
- csp-helper

## Upgrade/Migration notes

## Removals

## Deprecations

- KaaS v1 (k8s-cluster-api-provider)

## Security Fixes

During the R7 development cycle a few security issues were reported and we issued security
advisories and addressed them via maintenance updates. All of these issues are also fixed
in the upcoming R7 release. These include:

* A [Security Advisory on arbitrary file access through QCOW2 external data file (CVE-2024-32498)](https://scs.community/security/2024/07/02/cve-2024-32498/)
* A [Security Advisory on incomplete QCOW2 and VMDK image handling protections (CVE-2024-40767)](https://scs.community/security/2024/07/23/cve-2024-40767/)

## Resolved Issues

## Standards Conformance

## Release Tagging

## List of known issues & restrictions in R7

### KaaS

As alread in R6, we do not yet have the handling of restrictive security groups
implemented nor the ability to avoid OpenStack scheduling more than one control
plane node on the same host (hypervisor).
Those features are in review upstream.

## Contributing

We appreciate contribution to strategy and implementation, please join
our community -- or just leave input on the github issues and PRs.
Have a look at our [How to contribute page](https://scs.community/contribute/).

## Thanks
