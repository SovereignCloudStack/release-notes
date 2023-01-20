# Releases and Roadmap

## Release 0 (2021-07-15)

SCS R0 has been released on 2021-07-15 and bundles the work
accomplished by the community prior to the full start of the project.

See [Release Notes for R0](Release0.md) for more information.

## Release 1 (2021-09-29)

R1 came quickly after R0 and was the first release to ship a production ready k8s stack
(with k8s cluster API), some identity federation integration and much improved
preconfiguration for monitoring and logging.

See [Release Notes for R1](Release1.md) for more information.

## Release 2 (2022-03-23)

This release delivers vast improvements for bare metal automation
and the features in the container layers.

See [Release Notes for R2](Release2.md) for more information.

## Release 3 (2022-09-21)

Release 3 features user federation, increase in deployment and upgrade
velocity by improving automated test coverage as well as bringing disk encryption
based on tang from the state of a technical preview to be fully supported.

See [Release Notes for R3](Release3.md) for more information.

## Roadmap

We have a 6 month release cadence -- R4 will follow in March 2023.
Until then, we will provide bugfixes and security fixes for R3.

We do work towards a model where our partners can actually follow our main
development branches -- right now, our CI needs a bit more coverage though
to make this safe.

# Contribute and Connect

Please see the [SCS contributor guide](https://scs.community/docs/contributor/).

# Standards, Conformity and Certification

We intend to work on a conformity test suite.

Right now, we are basically relying on upstream tests -- 
[RefStack](https://refstack.openstack.org/) (to perform
the [OpenStack trademark certification](https://refstack.openstack.org/#/guidelines)
tests formerly known as DefCore) and the Kubernetes CNCF conformance tests run through
[sonobuoy](https://sonobuoy.io/).

We have two specific standards aligned within the SCS community (and have also
sought feedback from the broader Gaia-X and OpenStack communities):

* [SCS Flavor naming and standard flavors standard](Design-Docs/flavor-naming.md)

* [SCS Image naming and metadata standard](Design-Docs/Image-Properties-Spec.md)

Beyond this, we have a [draft document](Design-Docs/SCS-Spec.md) that captures our
view on how SCS compatible environments should look like. This one has not yet
seen sufficient review to be eligible for standardization. However, we appreciate
feedback (raise issues and PRs or start discussions).

# Issues and bugs

Please raise issues on github. If you can identify the affected component,
raise the issue against the relevant repository in the SovereignCloudStack
or OSISM space. Otherwise you can use
the [issues repository](https://github.com/SovereignCloudStack/issues).
Obviously we appreciate PRs even more than issues;
please don't forget to sign off your contributions (see
[contributor guide](https://scs.community/docs/contributor/) ).

When reporting bugs, it is very useful to include some standard information
typically needed to analyze:
* What state of software (SCS) were you testing? What version numbers ... ?
* How does your environment look like (hardware, operating systems, etc.)?
* What did you do?
* What did you expect? What happened instead?
* Have you done this successfully before? What changed?
* Can this be reproduced? Occasionally? Reliably? How?
* Any analysis you have done? Experiments and their results? Log files?


# Other resources

Please check our main [web page](https://scs.community/).
If you are an onboarded SCS community member, find here a link to our
[nextcloud](https://scs.sovereignit.de/) (login required).

Our community interacts through our [github organization](https://github.com/sovereignCloudStack/),
on [mailing lists](https://scs.sovereignit.de/mailman3/postorius/lists/) as well as
chats [matrix.org:SCS](#scs-general:matrix.org).

