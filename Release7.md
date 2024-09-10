# Release Notes for SCS Release 7

Release 7 has been published on September 11, 2024.

## Scope

We had stated the goals of previous releases in terms of desired outcomes. For R7, we built on top of
[R6](https://docs.scs.community/docs/releases/Release6#scope) and continued in the same direction.

With R7 being the last release during the original funded SCS project, we put a lot of focus on finalizing
the work on many of the components. Major progress was achieved for example in the area of observability
(status page, new health monitor) and of course again in the cluster stacks, our Kubernetes as a Service
framework. Finalizing also means a lot of non-feature work, such as ensuring that tests exist and pass,
documentation, guides and blog articles are written and upstream contributions are done. An extra
focus was put on integrating the components well with each other and documenting the performance of a
complete SCS deployment better. One result of this is the [turnkey solution overview table](Link:TODO),
another one the Technical Preview for our
[Central API](https://scs.community/tech/2024/08/13/central-api-tech-preview-release/) where we
demonstrate our vision of one API for all of the SCS stack in a usable form.

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

SCS KaaS v2, better known as SCS Cluster Stacks are the new reference implementation for managing SCS compliant Kubernetes on SCS compliant infrastructure.
While we recommend to build Cluster Stacks for own purposes, the SCS projects provides preassembled Cluster Stacks to try out on OpenStack. These are simply called `SCS` and come with a number of configuration options, which can be found in the [SCS Docs](https://docs.scs.community/docs/container/components/cluster-stacks/components/cluster-stacks/providers/openstack/configuration).

#### Host Cluster Stack release assets on an OCI registry

With the first versions of SCS Cluster Stacks the only way to host the release assets was GitHub Releases. This made cross-team testing harder and lead to an unusual amount of releases on specific GitHub repositories.  
With SCS R7 it will be possible to put publish the Cluster Stack release assets on an OCI registry.

#### Build node images with csctl for use in openstack

The [csctl-plugin-openstack](https://github.com/SovereignCloudStack/csctl-plugin-openstack) can now be used to build node images and upload them directly to Swift Object Store.

## New Features (Highlights)

### Operator focused improvements

### IaaS

* The Prometheus node-exporter is now deployed on all nodes by default.
* cAdvisor is now deployed on all nodes by default.
* It is now possible to manage custom images with `osism manage images`.
* It is now possible to manage custom flavors with `osism manage flavors`.
* Various commands for disabling nodes, such as `osism apply disable-compute-node` have been added.
* With the osism manage image octavia command it is possible to rotate the Octavia Amphora image, which is rebuilt daily.
* With the osism manage image clusterapi command it is possible to import all currently stable Cluster API images


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

* Documentation for IaaS pentesting: https://docs.scs.community/docs/operating-scs/components/automated-pentesting-iaas/overview/
* Documentation for KaaS pentesting: https://docs.scs.community/docs/operating-scs/components/automated-pentesting-kaas/overview/


### Tech Preview: SCS Health Monitor

As announced in the Release Notes for R6, a new health monitor was in the works during the last months. With R7 we are now at the state of releasing the first Tech Preview of the new SCS Health Monitor. It not only covers the infrastructure-as-a-service layer, but also has workflows for Kubernetes tests.

While the SCSHM took many IaaS-related inspirations from the OSHM, the developer team for the new SCSHM has built an entirely new foundation for behavior-driven testing in SCS. The implementation is based on the Python `behave` framework, using the Gherkin language for test definitions. Using Python allows the tool to then use Python-native API cients for OpenStack and Kubernetes, with room for extension for many other tools in SCS as well.

During the test run, metrics and results are collected and exported via a Prometheus Push Gateway into a Grafana-based dashboard.

* Documentation: https://docs.scs.community/docs/category/scs-health-monitor
* Repository: https://github.com/SovereignCloudStack/scs-health-monitor


### Status Page: 1.0 Full Release

The Status Page, [formerly available as a tech preview](https://scs.community/de/tech/2024/04/30/status-page-tech-preview-release/), has now been feature-complete for its first full release as version 1.0.

The status page is the web frontend to our status API server, a record keeper of any incidents that may limit or break parts of your SCS infrastructure. While the tech preview version was limited to displaying this data, this full release contains not just improvements to the code working in the background, but also two new features:

* The new *Maintenance Events* are a type of incident that are set in the future and allow you to announce planned times of limited or non-existant service well in advance, right on the status page.
* The *Management Page* provides a convenient interface to create and update incidents and maintenance events using a web UI, secured behind an authorization system using OpenID.

The [status-page-deployment repository](https://github.com/SovereignCloudStack/status-page-deployment) and [its documentation](https://docs.scs.community/docs/operating-scs/components/status-page-deployment/docs/overview) contain full example and development setups. Check the [Quickstart Guide](https://docs.scs.community/docs/operating-scs/components/status-page-deployment/docs/quickstart) to see what you need to run your own API server with status page or visit the SCS project's [publicly hosted installation](https://status.k8s.scs.community/) to get an impression.

* `status-page-openapi` (concepts)
  * [Repository](https://github.com/SovereignCloudStack/status-page-openapi)
  * [Documentation](https://docs.scs.community/docs/category/concepts)
* `status-page-api` (API server reference implementation)
  * [Repository](https://github.com/SovereignCloudStack/status-page-api)
  * [Documentation](https://docs.scs.community/docs/category/api)
* `status-page-deployment` (examples and templates, including a development setup based on kind)
  * [Repository](https://github.com/SovereignCloudStack/status-page-deployment)
  * [Documentation](https://docs.scs.community/docs/operating-scs/components/status-page-deployment/docs/overview)
* `status-page-web` (frontend code and development instructions)
  * [Repository](https://github.com/SovereignCloudStack/status-page-web)
  * [Documentation](https://docs.scs.community/docs/category/web)

Also see the [Tech preview announcement post](https://scs.community/de/tech/2024/04/30/status-page-tech-preview-release/).

### KaaS

#### Multi-Stage-Addons support in Cluster Stack Operator

The Multi-Stage-Addons mark a key feature of the Cluster Stacks approach. It enables the Cluster Stack Operator to upgrade clusters in predefined ways using Cluster API Lifecycle Hooks. Cluster upgrades can be painful, if e.g. new Kubernetes versions won't support the current used CNI. With the Multi-Stage-Addons the order of upgrades can be defined in a specific order which ensures the compatibility of components.

#### Tech Preview: Cluster Gen - GUI for generating Cluster objects

As a user of a ClusterAPI based managed Kubernetes the interface is the `Cluster` object. To define this can be complicated and depends on the used ClusterClass. With [Cluster-Gen](https://github.com/SovereignCloudStack/cluster-gen/) the possible options will be pulled from an existing ClusterClass to provide a form which can be used to create the `Cluster` object to apply it to the management cluster, which results in the usable workload cluster.

#### Tech Preview: Kamaji

Kamaji is a Control Plane manager which makes it possible to host Control Planes on pods instead on dedicated VMs. This makes them highly scalable and safes a lot of resources. We now provide Cluster Stacks using Kamaji as Control Plane provider, which can be especially interesting for smaller CSPs.

#### Openstack csp helper

The [openstack-csp-helper](https://github.com/SovereignCloudStack/openstack-csp-helper) is a simple Helm Chart to build and deploy the needed secrets used in the mangement cluster (to create the machines) and in the workload cluster (to get persistent volumes and loadbalancers) from a usual `clouds.yaml` file.

### IAM

The keycloak container has been updated and the build process has seen further automation.
THe container is now deployed into the internal infrastructure k3s, using the modern
Kubernetes-based interfaces that OSISM has provided to allow providers to plug-in
additional components at the management layer.

The work that SCS has done on a clean role model including the domain manager role
needed for self-service user and role management has successfully been discussed
upstream and contributed there. This means that the SCS downstream implementation
can be dropped and replaced by the upstream one in the next release R8.

## Upgrade/Migration notes

## Removals

* The following roles have been removed from OSISM, most of them do not affect the SCS operators in any real matter:
  * `osism.services.bird`
  * `osism.commons.kompose`
  * `osism.services.openldap`
  * `osism.services.openstack_health_monitor`
  * `osism.validations.refstack`


## Deprecations

- KaaS v1 (k8s-cluster-api-provider)

## Security Fixes

During the R7 development cycle a few security issues were reported and we issued security
advisories and addressed them via maintenance updates for R6 (and some also R5). All of these
issues are also fixed in the upcoming R7 release. These include:

* A [Security Advisory on arbitrary file access through QCOW2 external data file (CVE-2024-32498)](https://scs.community/security/2024/07/02/cve-2024-32498/)
* A [Security Advisory on incomplete QCOW2 and VMDK image handling protections (CVE-2024-40767)](https://scs.community/security/2024/07/23/cve-2024-40767/)
* A [Security Advisory on Image Processing in Ironic](https://scs.community/security/2024/09/07/cve-2024-44082/)

<!--
## Resolved Issues
-->
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

OSISM uses the tag 8.0.0 for SCS R7 and some of the related repositories in the SCS project
use a similar tag. Some repositories use maintained (stable) branches where the amount of changes
is limited to mainly bug and security fixes to make life easier for our operators that need to
stay on top of our security fixes without incurring too much change. Please note that by default,
we will stop maintaining the old R6 branches after October. Also note that we have been
able to establish a larger number of smaller changes during the R6 lifecycle to facilitate
the increasing insight that 4 (well-tested) small changes are less risky that one step with
3x the amount of changes. We will continue to work with our operators to ensure there is a
good balance between effort (4 small changes is more operational effort!) and risk.

## List of known issues & restrictions in R7

### IAM

The most underestimated topic for SCS as probably the creation of a self-service federated
Identity- and Access Management. While we were able to connect an external Identity Provider
via OpenID Connect and fix the issues with API access before, we still have not achieved
the full vision with customers being able to configuring everything via self-service.
Our work on the domain-manager role (persona) in OpenStack has been contributed upstream, so
we have an aligned view on this finally. Unfortunately, we will only get the results of
this into the next release (R8) when we use OpenStack 2024.2 (Dalamatian). Discussing with
our operators, we also learned about new requirements where *some* user identities need to
work across several tenants and thus escape our simple view of users always belonging to
a tenant. This topic will keep us busy for the next few releases.

### KaaS

As already in R6, we do not yet have the handling of restrictive security
groups implemented nor the ability to avoid OpenStack scheduling more than one
control plane node on the same host (hypervisor).
Those features are in review upstream.

### OVN Loadbalancers

The octavia ovn load balancer provider has a file descriptor leak which can lead
to a dysfunctional octavia-api service. The [bug](https://github.com/osism/issues/issues/959)
is still in analysis; for now,
restarting the octavia-api service before the exhaustion is the workaround.

## Contributing

We appreciate contribution to strategy and implementation, please join
our community -- or just leave input on the github issues and PRs.
Have a look at our [How to contribute page](https://scs.community/contribute/).

## On to R8 ...

We expect the continuation of our half-yearly cycle and thus an R8 in March. A lot of the work on the
SCS components was done via tenders paid for and managed by the OSBA (with the funding from the german
government / BMWK). The inclusion and exclusion of software components in future releases will depend
more on the willingness of the component maintainers than on the setup of the SCS work packages; we
anticipate more projects to seek alignment with SCS while existing projects can more easily work
with loser coupling. We will make this transparent to the community in order to ensure that we
maintain predictability for operators that rely on our technology.

## Thanks

Even more contribution from upstream communities and partners in the SCS ecosystem, too many
to mention here. Technology can only be improved to a certain degree in the hands of capable
development teams; the real push comes through usage and very well-working feedback loops
from operators and users back into engineering. Not everyone realizes it, but Open Source
allows for extremely direct feedback, as users have the full bandwidth of option from just
channeling a requirement or idea into the development via a full bug analysis up to code
contributions. So let us use the opportunity for calling out to our operators and users.
We have been working on growing the SCS adoption further and are looking forward to
further announcements during the next weeks.
