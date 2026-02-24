+++
title = 'Kubernetes'
date = 2024-02-11

+++

### What problems have you actually solved with kubernetes?

- Immutable infrastructure - leads to things like better security- e.g scan 1 container image instead of every os image
- Rapid deploys (cm starts were much slower than container startups)
- Elastic horizontal scaling
- End to end in transit encryption with a mesh.
- Some really useful non disruptive, seamless deployment patterns that are low effort to implement - blue / green, canary.
- Lots of standardization, tooling and vendor support, e.g from cloud providers
- A lot of these are not kubernetes specific - some are possible in other container environments, vm, or physical environments, but the consistency, speed and support in k8s are pretty favourable imo.
- Running thousands of services and jobs across dozens of regions in three cloud providers for a Fortune 500 company, while providing consistent compliance, logging, monitoring, tracing, network policy, durability, and deployment capabilities across all clouds.
