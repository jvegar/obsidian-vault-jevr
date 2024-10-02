#kubernetes 
When you deploy Kubernetes, you get a cluster.

A **Kubernetes cluster** consists of a set of **worker machines**, called [nodes](https://kubernetes.io/docs/concepts/architecture/nodes/), that run containerized applications. Every cluster has at least one worker node.

The worker node(s) host the [Pods](https://kubernetes.io/docs/concepts/workloads/pods/) that are the components of the application workload. The [control plane](https://kubernetes.io/docs/reference/glossary/?all=true#term-control-plane) manages the worker nodes and the Pods in the cluster. In production environments, the control plane usually runs across multiple computers and a cluster usually runs multiple nodes, providing fault-tolerance and high availability.

This document outlines the various components you need to have for a complete and working Kubernetes cluster.
![[attachments/components-of-kubernetes.svg]]
## Control Plane Components[](https://kubernetes.io/docs/concepts/overview/components/#control-plane-components)

The control plane's components ***==make global decisions about the cluster==*** (for example, scheduling), as well as ***==detecting and responding to cluster events==*** (for example, starting up a new [pod](https://kubernetes.io/docs/concepts/workloads/pods/) when a Deployment's `[replicas](https://kubernetes.io/docs/reference/glossary/?all=true#term-replica)` field is unsatisfied).

Control plane components can be run on any machine in the cluster. However, for simplicity, setup scripts typically start all control plane components on the same machine, and do not run user containers on this machine. See [Creating Highly Available clusters with kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/) for an example control plane setup that runs across multiple machines.
### kube-apiserver[](https://kubernetes.io/docs/concepts/overview/components/#kube-apiserver)

The API server is a component of the Kubernetes [control plane](https://kubernetes.io/docs/reference/glossary/?all=true#term-control-plane) that ***==exposes the Kubernetes API==***. The API server is the ***==front end for the Kubernetes control plane==***.
The main implementation of a Kubernetes API server is [kube-apiserver](https://kubernetes.io/docs/reference/generated/kube-apiserver/). kube-apiserver is designed to scale horizontally—that is, it scales by deploying more instances. You can run several instances of kube-apiserver and balance traffic between those instances.

### etcd[](https://kubernetes.io/docs/concepts/overview/components/#etcd)

Consistent and highly-available ***==key value store==*** used as Kubernetes' ***==backing store for all cluster data==***.
If your Kubernetes cluster uses etcd as its backing store, make sure you have a [back up](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster) plan for the data.
You can find in-depth information about etcd in the official [documentation](https://etcd.io/docs/).

### kube-scheduler[](https://kubernetes.io/docs/concepts/overview/components/#kube-scheduler)

Control plane component that ***==watches for newly created [Pods](https://kubernetes.io/docs/concepts/workloads/pods/) with no assigned [node](https://kubernetes.io/docs/concepts/architecture/nodes/), and selects a node for them to run on==***.
Factors taken into account for scheduling decisions include: individual and collective resource requirements, hardware/software/policy constraints, affinity and anti-affinity specifications, data locality, inter-workload interference, and deadlines.

### kube-controller-manager[](https://kubernetes.io/docs/concepts/overview/components/#kube-controller-manager)

Control plane component that ***==runs [controller](https://kubernetes.io/docs/concepts/architecture/controller/) processes==***.

Logically, each [controller](https://kubernetes.io/docs/concepts/architecture/controller/) is a separate process, but to reduce complexity, they are all compiled into a single binary and run in a single process.

There are many different types of controllers. Some examples of them are:

- Node controller: Responsible for ***==noticing and responding when nodes go down==***.
- Job controller: ***==Watches for Job objects==*** that represent one-off tasks, then creates Pods to run those tasks to completion.
- EndpointSlice controller: ***==Populates EndpointSlice objects==*** (to provide a link between Services and Pods).
- ServiceAccount controller: ***==Create default ServiceAccounts==*** for new namespaces.

The above is not an exhaustive list.

### cloud-controller-manager[](https://kubernetes.io/docs/concepts/overview/components/#cloud-controller-manager)

A Kubernetes [control plane](https://kubernetes.io/docs/reference/glossary/?all=true#term-control-plane) component that ***==embeds cloud-specific control logic==***. The [cloud controller manager](https://kubernetes.io/docs/concepts/architecture/cloud-controller/) lets you ***==link your cluster into your cloud provider's API==***, and separates out the components that interact with that cloud platform from components that only interact with your cluster.

The cloud-controller-manager only runs controllers that are specific to your cloud provider. If you are running Kubernetes on your own premises, or in a learning environment inside your own PC, the cluster does not have a cloud controller manager.

As with the kube-controller-manager, the cloud-controller-manager combines several logically independent control loops into a single binary that you run as a single process. You can scale horizontally (run more than one copy) to improve performance or to help tolerate failures.

The following controllers can have cloud provider dependencies:

- Node controller: For checking the cloud provider to determine if a node has been deleted in the cloud after it stops responding
- Route controller: For setting up routes in the underlying cloud infrastructure
- Service controller: For creating, updating and deleting cloud provider load balancers

## Node Components[](https://kubernetes.io/docs/concepts/overview/components/#node-components)

Node components run on every node, maintaining running pods and providing the Kubernetes runtime environment.
### kubelet[](https://kubernetes.io/docs/concepts/overview/components/#kubelet)

An agent that runs on each [node](https://kubernetes.io/docs/concepts/architecture/nodes/) in the cluster. It ***==makes sure that [containers](https://kubernetes.io/docs/concepts/containers/) are running in a [Pod](https://kubernetes.io/docs/concepts/workloads/pods/)==***.
The [kubelet](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/) takes a set of ***PodSpecs*** that are provided through various mechanisms and ensures that the containers described in those ***PodSpecs*** are running and healthy. The kubelet doesn't manage containers which were not created by Kubernetes.

### kube-proxy[](https://kubernetes.io/docs/concepts/overview/components/#kube-proxy)

kube-proxy is a ***==network proxy==*** that runs on each [node](https://kubernetes.io/docs/concepts/architecture/nodes/) in your cluster, ***==implementing part of the Kubernetes [Service](https://kubernetes.io/docs/concepts/services-networking/service/) concept==***.

[kube-proxy](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/) ***maintains network rules*** on nodes. These network rules ***allow network communication*** to your Pods from network sessions inside or outside of your cluster.

kube-proxy uses the operating system packet filtering layer if there is one and it's available. Otherwise, kube-proxy forwards the traffic itself.

### Container runtime[](https://kubernetes.io/docs/concepts/overview/components/#container-runtime)

A fundamental component that ***==empowers Kubernetes to run containers effectively==***. It is responsible for ***==managing the execution and lifecycle of containers==*** within the Kubernetes environment.

Kubernetes supports container runtimes such as [containerd](https://containerd.io/docs/), [CRI-O](https://cri-o.io/#what-is-cri-o), and any other implementation of the [Kubernetes CRI (Container Runtime Interface)](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-node/container-runtime-interface.md).