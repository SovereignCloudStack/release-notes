# Release Notes for SCS Release 7

This document is work in progress for the upcoming Release 7.
Release 7 will be published on September 4th 2024.
This note will be removed, once Release X is released and these notes are valid.

## Scope

## Component Versions and User-visible improvements (highlights)

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


## Upgrade/Migration notes

## Removals

## Deprecations

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
