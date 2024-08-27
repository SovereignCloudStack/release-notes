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

#### Cluster Stack `SCS`

SCS KaaS v1, better known as SCS Cluster Stacks are the new reference implementation for managing SCS compliant Kubernetes on SCS compliant infrastructure.
While there is the possibility to build Cluster Stacks for own purposes, the SCS projects provides preassembled Cluster Stacks for immediate use. These are simply called `SCS` and come with a number of configuration options, see https://github.com/SovereignCloudStack/cluster-stacks/blob/main/docs/providers/openstack/configuration.md.

#### Host Cluster Stack release assets on an OCI registry

With the first versions of SCS Cluster Stacks the only way to host the release assets was GitHub Releases. This made cross-team testing harder and lead to an unusual amount of releases on specific GitHub repositories.  
With SCS R7 it will be possible to put publish the Cluster Stack release assets on an OCI registry.

#### Build node images with csctl for use in openstack

The [csctl-plugin-openstack](https://github.com/SovereignCloudStack/csctl-plugin-openstack) can now be used to build node images and upload them directly to Swift Object Store.

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

#### Multi-Stage-Addons support in Cluster Stack Operator

The Multi-Stage-Addons mark a key feature of the Cluster Stacks approach. It enables the Cluster Stack Operator to upgrade clusters in predefined ways using Cluster API Lifecycle Hooks. Cluster upgrades can be painful, if e.g. new Kubernetes versions won't support the current used CNI. With the Multi-Stage-Addons the order of upgrades can be defined in a specific order which ensures the compatibility of components.

#### Tech Preview: Cluster Gen - GUI for generating Cluster objects

As a user of a ClusterAPI based managed Kubernetes the interface is the `Cluster` object. To define this can be complicated and depends on the used ClusterClass. With [Cluster-Gen](https://github.com/SovereignCloudStack/cluster-gen/) the possible options will be pulled from an existing ClusterClass to provide a form which can be used to create the `Cluster` object to apply it to the management cluster, which results in the usable workload cluster.

#### Tech Preview: Kamaji

Kamaji is a Control Plane manager which makes it possible to host Control Planes on pods instead on dedicated VMs. This makes them highly scalable and safes a lot of resources. We now provide Cluster Stacks using Kamaji as Control Plane provider, which can be especially interesting for smaller CSPs.

#### Openstack csp helper

The [openstack-csp-helper](https://github.com/SovereignCloudStack/openstack-csp-helper) is a simple Helm Chart to build and deploy the needed secrets used in the mangement cluster (to create the machines) and in the workload cluster (to get persistent volumes and loadbalancers) from a usual `clouds.yaml` file.

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

This release brings quality-of-life improvements such as:

- improved documentation of the certification scopes: they now include formal parameters,
  such as the image spec file for the standard on standard images ([SCS-compatible IaaS
  v4](https://docs.scs.community/standards/scs-compatible-iaas/)),
- removal of unnecessary strictness from the standard
  [scs-0103-v1](https://docs.scs.community/standards/scs-0103-v1-standard-flavors/)
  on standard flavors, enabling operators to provide more meaningful information via `extra_specs`,
- improved output and documentation of the main check script,
- and bug fixes on the test scripts.

We are quite pleased that it could be shown in May that compliance with the certificate scope
SCS-compatible IaaS v4 can be achieved with an alternative implementation (other than our reference
implementation), namely Yaook,
[with reasonable effort](https://scs.community/de/2024/05/13/cost-of-making-an-openstack-cluster-scs-compliant/).

Finally, we are also happy that we could welcome new partners to our table of SCS clouds, namely AOV,
SysEleven, and, with proof-of-concept environments, the companies KDO Service GmbH and Cloud&Heat
Technologies GmbH.

## Release Tagging

## List of known issues & restrictions in R7

### KaaS

As already in R6, we do not yet have the handling of restrictive security
groups implemented nor the ability to avoid OpenStack scheduling more than one
control plane node on the same host (hypervisor).
Those features are in review upstream.

## Contributing

We appreciate contribution to strategy and implementation, please join
our community -- or just leave input on the github issues and PRs.
Have a look at our [How to contribute page](https://scs.community/contribute/).

## Thanks
