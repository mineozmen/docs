---
description: >-
  Our dedicated installation documentation provides step-by-step guidance and
  our AWS marketplace offering can get you started immediately
icon: desktop-arrow-down
---

# Installation

## Easy Access to Rierino

Fastest way to start testing and developing on Rierino is using our free **Community Edition** on AWS Marketplace. It is deployed as a multi-service VM. You can choose the region and instance type. You also get full control of the environment.

<mark style="background-color:yellow;">**Click**</mark> [<mark style="background-color:yellow;">**here**</mark>](https://aws.amazon.com/marketplace/pp/prodview-up2fcxku3k742) <mark style="background-color:yellow;">**to start with AWS now.**</mark>

### What you get with Community Edition

* A ready-to-use environment for exploring the platform.
* A good baseline for running MVPs and small projects.
* A fast path to validate the architecture before scaling up with a custom installation.

## Custom Installation

For installing Rierino yourself, follow the [Installation](https://app.gitbook.com/o/lZ6vN24S2tXuHH7DjdWa/s/pV7u8nn9fFM9XMp0tNic/) documentation. You can also check alternative ways to start using Rierino [here](https://rierino.com/start).

### Common installation options

* **Single VM:** use the Docker Compose setup in [Sandbox Deployment](https://app.gitbook.com/s/pV7u8nn9fFM9XMp0tNic/deployment-alternatives/sandbox-vm-deployment).
* **Kubernetes:** use the fully automated Helm Chart option in [Kubernetes Deployment](https://app.gitbook.com/s/pV7u8nn9fFM9XMp0tNic/deployment-alternatives/kubernetes-deployment).

The standard configuration uses **MongoDB** as the main data store. This is a good default for most training and initial development.

## Secrets and Configurations

Integrating Rierino with external systems, such as database solutions, enterprise systems and file systems require configuration of connection details as well as credentials. While it is possible to define such configurations directly within Rierino UI, it is recommended to keep these values inside external configuration files (especially for secrets). Depending on the deployment option the most common way to define these configurations are as follows:

* **Single VM:** globalconfig.properties and globalsecrets.properties files under admin\_runner folder are used
* **Kubernetes:** global-config ConfigMap and global-secrets Secret under admin-backend namespace are used
