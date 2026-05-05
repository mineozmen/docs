---
description: >-
  Deployments are sets of runners which are deployed on a specific machine, node
  or pod.
---

# Deployments

<figure><img src="../../../.gitbook/assets/image (108).png" alt=""><figcaption><p>Deployment UI</p></figcaption></figure>

Deployments allow grouping of runners into physical components, as well as configurations for deploying same runners on different infrastructure (e.g. different cloud providers, different containerization models) based on business and technical requirements.

In the most typical scenario, a deployment package bundles a few runners, specifies kubernetes deployment parameters such as namespace, node pool, rierino version and replica count, which results in deployment of a Java application using Rierino's in-cluster deployer web service and kubernetes job running ansible playbooks and helm charts. However, it is possible to 100% customize this deployment model, since the deployment activity is basically a Rierino backend API call with the deployment package ID, which can easily trigger a Jenkins job for an on-prem deployment, etc. It is even possible to use deployment packages for non-runner deployments (e.g. ui, gateway, etc.) with minimal customizations on the deployment API design.
