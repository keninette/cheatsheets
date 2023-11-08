---
tags:
  - tag-demo
---

[🏡 Home](../index.md) > [Learning](index.md) > GCP

# ☁️ Google Cloud Platform

## 🤓 Lexicon
- **On-demand resource provisioning / Elasticity** : only rent resources when you need them and release them when you don't.

## ⏯️ Compute engines (GCE)
[URL](https://circle.udemy.com/course/google-cloud-certification-associate-cloud-engineer/learn/lecture/25121490#overview)

Service used to provision & manage virtual machines withing the GCP environment:

- Create and manage lifecycle of VM instances.
- Load balancing & auto scaling.
- Attach storage to VM.
- Manage network connectivity & configuration.

### How to automatically set-up your VM ?

#### Bootstrapping
Use a startup script to execute when a VM is launched, you can configure it in the Automation section of the VM configuration. It will expect bash.

#### Instance template
- Use an instance template in which you can define:
    - The machine type.
    - The image.
    - Its labels.
    - A startup script.
    - Many other properties.
- They are used to create VM instances & managed instance groups.
- They cannot be updated, you will need to duplicate it in order to update it.

#### Custom image

### VM families

 - **General purpose (E2, N2, N2D, N1)** : apps & servers, small to medium db, dev environments.
 - **Memory Optimized (M2, M1)** : ultra-high memory workloads (analytics, large db).
 - **Compute Optimized (C2)** : compute intensive workloads (gaming).

Machine name: follows the pattern family-workload-nb_of_cores.

> Example:
> E2-standard-2 : E2 family, standard worload with 2 cores.

## 📚️ Sources
- [Udemy GCP course](https://circle.udemy.com/course/google-cloud-certification-associate-cloud-engineer/learn/lecture/25120004#overview)

