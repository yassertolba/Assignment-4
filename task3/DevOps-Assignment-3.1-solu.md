# DevOps Assignment 3.1

**Trainee Name: Yasser Ahmed**  
**Group: ALX2_SWD1_G1**  

## Table of Contents

## **Theory Questions (10 points)**

**a) Kubernetes Architecture**  
**b) Deployments and Services**  
**c) Scaling and Autoscaling**  

## a) Kubernetes Architecture

## **1. What are the core components of a Kubernetes cluster (e.g., master, node, etcd, kube-apiserver)? Briefly explain their roles**

A Kubernetes cluster commonly contains one or more master nodes and one or more worker nodes. Every Kubernetes cluster must have at least one worker node.

![Kubernetes architecture](kubernetes-architecture.png)  
****Figure 1.*** Kubernetes architecture*

Figure 1 shows a typical Kubernetes cluster, highlighting three main components: the master node (also known as the control plane), the worker node, and the cloud API provider.

The master node is where all cluster management and execution decisions reside; it can be thought of as the mind of the Kubernetes cluster.

The worker node is where the pods are created and run, and a pod is the Kubernetes component inside which your application runs.

The master node is responsible for managing the worker nodes.

## Master Node (Control Plane)

The kube-apiserver component is the key component of the control plane or master node and is responsible for all the interactions within and outside of it.

The Kubernetes cluster runs on top of either a physical or virtual environment.

![Master node components](master-node-components.png)
****Figure 2.*** Master node components*

### **What Does the Master Node Do?**

Master node components make scheduling and controlling decisions related to the cluster.

Simply put, the master node manages the whole Kubernetes cluster, which includes the worker nodes.

The following are a few examples of what master nodes do:

- Suppose a pod needs to be scheduled;
  - that scheduling decision will be made by the master node component.

- Suppose a pod is defined to have three replicas, but one pod has gone down.
  - The master node component will ensure that a new replica is created so that the specification of three replicas is met.

### **Master Node Components**

key components of a master node or control plane:  

- API server (kube-apiserver)
- Scheduler (kube-scheduler)
- Controller managers (kube-controller-manager)
- Data store (etcd)

---

#### **API Server (kube-apiserver)**

The `kube-apiserver` component **implements and exposes the Kubernetes API** and validates and configures data for the API objects.

The `kube-apiserver` component provides an entry point to the calls made using the Kubernetes REST API.

All the requests are targeted to `kube-apiserver`, whether coming from the worker node or any other channel.

The `kube-apiserver` component can be thought of as the front end of the Kubernetes master node.

`kube-apiserver` exposes an HTTP API, through which any component or client can communicate with the API server.

Most of the operations performed by the `kubectl` or `kubeadm` command-line interface use this Kubernetes API.

The Kubernetes API can also be accessed using REST calls.

**kube-apiserver is the only component of the control plane or master node with which the worker nodes interact.**

The `kube-apiserver` component can be **scaled horizontally** so that at any point in time you can deploy more kube-apiserver instances and load balance the incoming traffic between those kube-apiserver instances.

---

#### **Scheduler (kube-scheduler)**

The `kube-scheduler` component **contains all the scheduling algorithms** and makes all the scheduling-related decisions for the Kubernetes cluster.

whenever a request for a new pod or node comes in, the kube-scheduler component checks the current state of the Kubernetes cluster, checks if any limit is breached, etc., and accordingly makes the scheduling decision.

***Notes***

- The kube-scheduler component doesn’t launch the pod; it only selects the most suitable node for running the pod and writes the name of that node on the pod object.

- It is the kubelet component of the worker node that will pull the image, create the container, start the container, etc.

---

#### **Controller Manager (kube-controller-manager)**

The controller manager runs processes and controllers to regulate the state of the Kubernetes cluster.

They are continuous watch loops that compare the cluster’s existing state to the specified state, and in case of any mismatch between the states, corrective action is taken.

Suppose, for whatever reason, that a node comes down. The kube-controller-manager component will detect this failure and respond to it by rescheduling the pods running on that failed node to a healthy node.

The following are some types of kube-controller-managers:

- **Node controller:** Watch loop noticing and responding to node failures

- **Job controller:** Responsible for watching `job` and `cron-job` objects

- **Endpoint slice controller:** Responsible for managing `EndpointSlice`.

- **Service account controller:** Responsible for managing the `ServiceAccounts` inside `namespaces`

---

#### **Data store (etcd)**

etcd is a distributed key-value store that stores and manages all the vital data required for any distributed system to function.

Kubernetes is among the most popular use cases of etcd; in Kubernetes, etcd is used to store and manage the config data and data related to the Kubernetes cluster state.

etcd is not used to store any sort of client application data.

### **cloud-controller-manager**

The `cloud-controller-manager` component lets you manage your cloud provider’s API.

If you are not running your Kubernetes cluster on cloud infrastructure, then you might not need this component.

The way `kube-controller-manager` component runs controllers that manage and regulate core Kubernetes resources, and cloud-controller-manager runs controllers that manage and regulate cloud provider resources.

For example, if you want to use a cloud provider’s load balancer or persistent storage, it will be provisioned and managed using a cloud-controller-manager component.

The cloud-controller-manager component was added to the Kubernetes architecture as part of Kubernetes v1.6. Before that, its functionality was part of `kube-controller-manager`. This was separated to decouple the core Kubernetes design and code from the cloud providers.

---

## **Worker Node**

Worker node is a kubernetes cluster run containerized applications.

![Worker node components](Worker-node-components.png)
****Figure 3.*** Worker node components*

### Worker Node Components

Every worker node within a Kubernetes cluster must run three services:

- Node agent (kubelet)
- Node agent (kube-proxy)
- Container runtime

---

### **Node agent (kubelet)**

kubelet is the main node agent that runs on every worker node of the Kubernetes cluster.

The kubelet node agent enables the interfacing between kube-apiserver and the worker nodes.

This kubelet node agent is also responsible for managing pod requirements such as mounting volumes, starting containers, and reporting status.

A PodSpec, which is just a JSON or YAML object describing the specification of a pod, is the object on which kubelet depends to work.

kube-apiserver sends pod specifications to kubelet, and then it is the responsibility of kubelet to ensure that all the containers are running as per the requirements defined in that pod specification.

kubelet registers the node with the API server.

It is the kubelet component of the worker node that pulls the image (either from the image registry or from a pre-downloaded image from the local system), creates the container, and starts the container, as you can see in the following output.

kubelet does not manage containers that are not created by Kubernetes.

---

### **Node agent (kube-proxy)**

Each worker node runs a network proxy, known as kube-proxy, that is responsible for all networking-related things on the worker nodes.

kube-proxy is responsible for the maintenance of all the networking-related rules and uses them to enable pod communication from within and outside the cluster.

---

### **Container Runtime**

The container runtime is the piece of software that knows how to create and run containers from a container image.

Kubernetes needs an underlying container runtime to manage the life cycle of containers.

---

## **What is a pod in Kubernetes, and how does it differ from a Docker container?**

In Kubernetes, pods are the smallest and most fundamental deployable unit. Within a pod, you can find one or more containers, and all containers will share the network and storage associated with the pod.

### **How is a Pod Different from a Docker Container?**

| **Aspect**               | **Docker Container**                                                                 | **Kubernetes Pod**                                                                                   |
|--------------------------|-------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Scope**                | A single container runtime instance.                                                | A group of one or more containers that share resources and are managed as a single unit.            |
| **Networking**           | Each container has its own network namespace.                                       | Containers in a Pod share the same network namespace (IP and port space).                           |
| **Storage**              | Volumes can be attached to individual containers.                                   | Volumes are shared among all containers in the Pod.                                                 |
| **Lifecycle**            | Managed individually by Docker.                                                    | Managed collectively by Kubernetes. If one container fails, the entire Pod is affected.             |
| **Use Case**             | Ideal for running a single process or application.                                 | Ideal for running tightly coupled processes that need to share resources (e.g., app + logging agent).|
| **Orchestration**        | Docker alone does not provide orchestration capabilities.                          | Pods are orchestrated by Kubernetes, which handles scaling, scheduling, and self-healing.           |
| **Isolation**            | Containers are isolated from each other by default.                                | Containers in a Pod are not isolated and can communicate directly via `localhost`.                  |
| **Self-Healing**         | Not built-in. Requires external tools or manual intervention.              | Built-in. Automatically restarts failed containers and replaces failed Pods.                         |
| **Health Monitoring**    | No built-in health checks.                                                 | Uses liveness and readiness probes to monitor container health.                                       |
| **Failure Recovery**     | Containers remain stopped until manually restarted.                        | Automatically restarts containers and replaces Pods to maintain the desired state.                    |
| **Node Failures**        | No automatic recovery. Containers on a failed node are lost.               | Reschedules Pods from failed nodes to healthy nodes.                                                 |
| **Desired State**        | No concept of desired state. Containers run as configured.                 | Continuously ensures the actual state matches the desired state defined in the configuration.         |
| **Scalability**          | Limited to manual scaling or external tools.                               | Automatically scales Pods and ensures high availability through self-healing.                         |

---

## b) Deployments and Services

## **Explain the purpose of a Kubernetes deployment. How do deployments ensure high availability of applications?**

Deployment is a workload resource that provides declarative updates for pods and replica sets.

Deployment is the object that defines specifications for the deployment of your application.

Using the Deployment object, you describe the desired deployment state of your application, and then at runtime, a deployment controller (a component of the master node) will make sure that the actual state matches the desired state.

When you create a Deployment object for the first time, a new set of pods and a new ReplicaSet are created.

When you remove the Deployment object, all pods and replica sets are removed.

Deployment directly manages the ReplicaSet it creates.

