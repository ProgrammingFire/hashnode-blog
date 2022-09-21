## What is Helm: The package manager for Kubernetes

## What is Helm?
Helm is a package manager for Kubernetes, which makes it simple to take applications and services that are highly re-iterable, or used across a number of scenarios, and deploy them into your typical K8s cluster. Helm charts enable developers and operators to easily package, configure, and deploy applications and services on Kubernetes clusters. Helm packages are charts composed of multiple configuration files in YAML format, and templates that are rendered to a K8s cluster as Kubernetes manifest files. Helm deploys packaged applications into Kubernetes and structures them in charts. 

## What are the benefits of using Helm?
Helm will enable development and operations teams to package, configure, manage, and deploy applications quickly into a Kubernetes cluster. To simplify the process of applying packages, configuration, and deployments for applications and services from operators and developers in a Kubernetes Cluster, a package manager called Helm has been gathered. The Helm package manager allows us to bundle complex Kubernetes deployments in one single package, which can be installed by one single command. With that in mind, Helm as the package manager for Kubernetes is the primary way users are making their Kubernetes setups reusable. 

If you are not familiar with Helm, Helm helps users to deploy in a more consistent manner, packaging up all of the necessary resources needed to deploy in Kubernetes. By using Helm, you drastically improve the performance of Kubernetes, as well as making it easier to recreate successful deployments again and again. Even if you are a Devops person trying to deploy internal applications or 3rd-party applications, you should be using Helm as a package management tool for Kubernetes. If you have the need to orchestrate more than one Kubernetes resource, and if there are multiple clusters with different configurations, then you have a solid use case to leverage Helm. 

Even if you are not actually deploying your app using Helm charts, it is a good way to get acquainted with the details of creating Kubernetes manifests. You then handle charts through the client-side tool, which is called Helm (just like the core project). 

## What are some of the features of Helm?
To actually deploy anything from the chart, once installed from Helm, there are going to have to be files in a directory called templates, that together make up configuration details of the actual pod Kubernetes is running, including which containers should load, like the one shown in the sample. Helm is a packaged app containing all of your versioned, preconfigured app resources deployed as one single unit. Helm charts are packaged (like Debs and RPMs) that contain the pre-configured Kubernetes resources like ConfigMaps, Deployments, StatefulSet manifests, PersistentVolumes, and the changeable settings for those. 

Helm server called Tiller will help you interact with Helm client and Kubernetes APIs in order to do the package management tasks in Helm. A server companion component, Tiller, which runs in your Kubernetes cluster, listens to Helm commands, and manages configuration and distribution of software releases in your cluster. Helm, once again, templates everything, makes sure that everything works the way that it is supposed to, and pushes configurations upstream into your Kubernetes cluster, then manages uptime as a critical feature of the specific stack. 

## How to install Helm?

### From the Binary release
From the Binary Releases
Every release of Helm provides binary releases for a variety of OSes. These binary versions can be manually downloaded and installed.

Download your desired version

Unpack it 
```bash
tar -zxvf helm-v3.0.0-linux-amd64.tar.gz
```
Find the helm binary in the unpacked directory, and move it to its desired destination 
```bash
mv linux-amd64/helm /usr/local/bin/helm
```

### From Script
Helm also has an installer script that will automatically grab the latest version of Helm and install it locally.

You can fetch that script, and then execute it locally. It's well documented so that you can read through it and understand what it is doing before you run it.

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

OR

```bash
curlhttps://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

## How to install a package using Helm?
The `helm install` command syntax includes a release name, the path to the chart, and optional flags:

```bash
helm install [release-name] [chart] [flags]
```

### Step 1: Find or Create a Helm Chart
To install a helm chart, you either have to find it online or create a helm chart yourself. You can obtain them in online repositories or the Artifact Hub. For more information about adding Helm repositories, read how to add, update or remove a Helm repo.

Use the `helm repo add` command to add the helm repository containing the chart you wish to install:

```bash
helm repo add [repository-name] [repository-address]
```

Update the repositories on your system:

```bash
helm repo update
```

Use the helm search command to search for the charts in the local repositories:

```bash
helm search repo [chart]
```

### Step 2: Install a Chart with helm install Command
There are multiple ways to use the helm install command for installing helm charts. The most common is using the chart reference given in the NAME section of the helm search output.

For example, using the syntax explained in the section above, to install Jenkins you would type:

```bash
helm install jenkins jenkins/jenkins
```

The chart reference consists of a repository prefix and the chart name. In the example above, Helm searches for the chart jenkins in the repo named jenkins before proceeding with the installation.

There are multiple ways to tell Helm where to look for a chart. Aside from a chart reference, you can also provide:

The path to a packaged chart:
```bash
helm install jenkins ./jenkins-1.2.3.tgz
```

The path to a directory containing an unpacked chart:
```bash
helm install jenkins-deployment ./jenkins-archive
```

The absolute URL:
```bash
helm install jenkins https://example.com/charts/jenkins-1.2.3.tgz
```

The chart reference and the URL of the repository:
```bash
helm install --repo https://example.com/charts/ jenkins-deployment jenkins
```

## Conclusion
In conclusion, Helm is a package manager for Kubernetes that helps you manage Kubernetes applications. Helm simplifies Kubernetes application deployment and management. You can use Helm to find, install, upgrade, and rollback Kubernetes applications. If you like this article, please consider sponsoring me. If you have any questions, please ask them in the comments or on [Twitter](https://twitter.com/programmingfire). Thanks for reading!