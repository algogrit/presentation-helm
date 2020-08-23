layout: true

.signature[@algogrit]

---

class: center, middle

![Logo](assets/images/helm.png)

Gaurav Agarwal

---

class: center, middle

![Me](assets/images/me.png)

Software Engineer & Product Developer

Principal Consultant & Founder @ https://agarwalconsulting.io

ex-Tarka Labs, ex-BrowserStack, ex-ThoughtWorks

---
class: center, middle

## Defining the problem

---

## Overview

- Helm is like a package manager for Kubernetes

- Up to now, you’ve seen how it takes many files and resources to put together a full working application in Kubernetes. This gets messy and hard to manage releases. There is a high level of risk of bugs being introduced because files drift.

- Helm is intended to package an entire release up into a single artifact that can be managed as a single unit, archived, versioned, etc.

- Whereas Docker Hub is a place to find third party Docker images, [Helm hub](https://hub.helm.sh/) kind of acts the same way.

- But Helm is there to help you manage, install, upgrade complex Kubernetes applications.

- Note that Helm 3 was released in late 2019 and changed things dramatically. If you’ve used Helm before, you’ll need to refresh your knowledge.

---

## Installation & Setup

- Install the Helm CLI onto your workstation

### For Mac

```bash
brew install helm
```

### For others

[Helm Install](https://helm.sh/docs/intro/install/)

---

## Main concepts/components

- **helm** - cli tool to manage helm

- **Charts** – a Helm package

- **Repository** – a collection of Charts

- **Release** – an instance of a Chart running in a cluster

- **Tiller** - managing your application on the cluster. *Removed in Helm 3.*

---
class: center, middle

### cli

---

- When you install Helm on your workstation, it comes with its own CLI.

- All commands start with helm

- If you already have kubectl installed and configured, them the Helm CLI will pick up on that configuration to know what cluster to point to.

- We can use the following common commands:

```bash
helm install <chart name>

helm list # show charts installed in our cluster

helm get <chart name> # get details on an installed chart

helm delete <chart name>
```

---
class: center, middle

### Charts

---

- Charts are the core concept in Helm

- For complex applications, you may have a whole collection of resource files. Deployments, Pods, ReplicaSets, Secrets, Volumes, DaemonSets, etc etc etc.

- To manage all this by hand means installing and managing lots of files and resources.

- Charts let you package everything together into one versioned archive for deployment.

- The files for a Chart are organized into a directory with the Chart name. It has a predetermined file structure:

```bash
Chart.yaml # the Chart information
Values.yaml # default configuration values
charts/ # a directory of dependent charts
templates/ # a directory of templates that, when combined with values, will generate valid Kubernetes resource files.
# Other optional files like notes, readme, license, etc.
```

---

- The charts directory contains a collection of manifest files. But in Helm, we can use variable files to inject values into the manifests.

- This is very powerful, allowing us to maintain a collection of values files that differ based on environment for example. By simply choosing a different values file, we can completely change the installation.

- The files in the templates directory use the files in the values directory to create complete manifests that get sent to the cluster.

- When developing templates, you can run them through the templating engine without sending them to the cluster. This will output the templates to the screen.

```bash
helm install --debug --dry-run <chart-name> ./directory
```

---
class: center, middle

### Repositories

---

- After a chart is developed, it can be packaged into a `.tgz` archive.

- From here, it can be pushed up to Helm Hub, or your own repository.

- A repository works like Docker Hub.

- You can also stand up your own private Helm repository.

- Helm Hub is the public repository at `hub.helm.sh`

---

#### Add a chart repository

```bash
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```

---

#### Installing a chart

- It’s a good idea to update the repo before installing any charts.

```bash
helm repo update
```

Then to install, just use:

```bash
helm install <chart name>
```

---

### Changes from Helm 2

#### Tiller was removed

- If you’re familiar with Helm 2, you may know about the in-cluster component called Tiller.

- Tiller was there to do the work of managing your application on the cluster.

- In Helm 3, Tiller was pushed overboard. RBAC being enabled by default on Kubernetes 1.6+ meant that dealing with Tiller was difficult. So the Helm team did away with it.

- Without Tiller, Helm is radically simplified.

---

#### Demo: `postgresql`

Let’s use the example of Postgres. Postgres has many components to be able to run on Kubernetes:

- `ConfigMaps`
- `Secrets`
- `PersistentVolumes`
- `Pods`
- `ReplicaSets`
- `Deployments`
- `Services`

Rather than put together the resource files for all these ourselves, which is time consuming and requires knowledge of the inner workings of Postgres, we can instead just install the Helm Chart for Postgres.

---
class: center, middle

#### Exercise: Install [dashboard](https://hub.helm.sh/charts/k8s-dashboard/kubernetes-dashboard) chart

---
class: center, middle

#### Demo: Creating a chart from scratch (Optional)

---

#### Exercise: Creating a chart (Optional)

- Run `helm create mychart`
- A `mychart` directory will be created. Open the `Chart.yaml` file in an editor.
- As you edit the chart, you can use `helm lint` to ensure it is valid. Try editing the version key.
- To package your chart, run `helm package mychart`. You’ll get a .tgz file with the version number.
- Install the chart into your cluster with `helm install mychart ./mychart-0.1.0.tgz`

---

class: center, middle

Code
https://github.com/algogrit/presentation-helm

Slides
https://helm.slides.algogrit.com
