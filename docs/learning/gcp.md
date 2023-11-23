---
tags:
  - tag-demo
---

[🏡 Home](../index.md) > [Learning](index.md) > GCP

# ☁️ Google Cloud Platform

## 🤓 Lexicon
- **On-demand resource provisioning / Elasticity** : only rent resources when you need them and release them when you don't.
- **Spot VMs (formerly known as Preemptible instances)**: short-lived compute instances. Very cheap, can be stopped anytime by GCP in a 30 seconds warning. Used for asynchronous tasks that are fault tolerant. 

## ⏯️ Compute engines (GCE)
[URL](https://circle.udemy.com/course/google-cloud-certification-associate-cloud-engineer/learn/lecture/25121490#overview)

Service used to provision & manage virtual machines withing the GCP environment:

- Create and manage lifecycle of VM instances.
- Load balancing & auto scaling.
- Attach storage to VM.
- Manage network connectivity & configuration.

Whenever a virtual machine is created, GCE also creates a hard disc to run the VM on. The disk will have the same name than the instance.

### VM families

- **General purpose (E2, N2, N2D, N1)** : apps & servers, small to medium db, dev environments.
- **Memory Optimized (M2, M1)** : ultra-high memory workloads (analytics, large db).
- **Compute Optimized (C2)** : compute intensive workloads (gaming).

Machine name: follows the pattern family-workload-nb_of_cores.

> Example:
> E2-standard-2 : E2 family, standard workload with 2 cores.

### How to automatically set-up your VM ?

#### Bootstrapping
Use a startup script to execute when a VM is launched, you can configure it in the Automation section of the VM configuration. It will expect bash.

#### Instance template
- Use an instance template in which you can define:
    - The machine type.
    - The image, or an image family which will use the latest non deprecated image in this family (example debian-9).
    - Its labels.
    - A startup script.
    - Many other properties.
- They are used to create VM instances & managed instance groups.
- They **cannot be updated**, you will need to duplicate it in order to update it.
- They are unaware of regions or zones, it's just a template.
- They can be overrided when used in an instance creation, they act as a setting of default values.

In order to create an instance template : 
`Compute Engine > Instance templates > Create`.

#### Custom image

Problem: installing OS patches, softwares at launch of VM instances increases boot up time.
Solution: create a custom image where all of these are already installed & use it in our template.

- They can be created from an instance that's already running (that's called **hardening** an image), a persistent disk, a snapshot, another image or a file in Cloud Storage.
- They can be shared across projects.
- An image can be marked as deprecated and a replacement image can be suggested.

##### How to harden an image ?
- Go to `GCE > Disks` & find the disk associated to your instance (remember, it has the same name). Click the ... menu & select "Create image".

>‼️ When you harden an image, you have to make sure that you choose the correct location. It can either be multi-regional, or confined to a specific region (which means you won't be able to create instances in another region with this custom image).

> You should never create an image from a running instance, it should always be stopped before. Google will remind you of it if you're trying to do it anyway.

All your images are stored in `GCE > Storage > Images`. From there, you can :
- Create an instance directly from this image (but you'll need to redefine everything you could already have defined in a template).
- Duplicate an existing template, specify the boot disk & select your custom image. In this case, you'll get all your customization & your custom-image.


### 🐞 Troubleshooting with Apache on a GCP VM
[URL](https://circle.udemy.com/course/google-cloud-certification-associate-cloud-engineer/learn/lecture/25121436#overview)

This is very specific, just watch the video.

### 💸 Managing costs
[URL](https://circle.udemy.com/course/google-cloud-certification-associate-cloud-engineer/learn/lecture/25121424#overview)

This is very specific too, just go the dedicated section.

### Live migrations
- They keep your VMs up & running when a host needs to be updated (software or hardware).
- Not available for GPU instances or Spot VMs.
- They need to be configured in the **availability policy while creating an instance**:
    - What happens on host maintenance ? Migrate or terminate ?
    - Should your instance be automatically restarted if they are terminated by anything else but user action.

## 📚️ Sources
- [Udemy GCP course](https://circle.udemy.com/course/google-cloud-certification-associate-cloud-engineer/learn/lecture/25120004#overview)

