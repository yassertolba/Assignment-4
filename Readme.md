# Assignment 4 - Summary

This repository contains solutions for **Assignment 4**, which is divided into three tasks. Each task focuses on different aspects of Kubernetes and DevOps practices. Below is a summary of the tasks and their respective solutions.

---

## Folder Structure

```structure
Asssignment-4/
└── Task1/
|   ├── Task_1_Solu.md
|
└── Task2/
|   ├── task-2-solu.md
|
└── Task3/
   ├── task-3-solu.md
   |
   ├── DevOps-Assignment-3.1-solu.md
```

---

## Task 1: Kubernetes Basics

**File:** `Task_1_Solu.md`

This task focuses on fundamental Kubernetes operations, including pod creation, ReplicaSets, Deployments, and scaling. The solution covers the following:

1. Installing a Kubernetes cluster using Minikube.
2. Creating pods (`redis` and `nginx`) using YAML definitions.
3. Checking pod status and updating pod images.
4. Creating and scaling ReplicaSets.
5. Creating and managing Deployments.
6. Rolling back deployments and understanding deployment strategies.
7. Creating a deployment with specific labels and replica counts.

---

## Task 2: Advanced Kubernetes Concepts

**File:** `task-2-solu.md`

This task delves into advanced Kubernetes concepts such as DaemonSets, Services, and static pods. The solution includes:

1. Querying and creating DaemonSets.
2. Deploying pods with specific labels and exposing them via Services.
3. Creating and accessing a NodePort service.
4. Investigating static pods and their distribution across nodes.
5. Testing service accessibility using `curl`.

---

## Task 3: ConfigMaps, Secrets, and Persistent Storage

**File:** `task-3-solu.md`

This task explores Kubernetes ConfigMaps, Secrets, multi-container pods, and persistent storage. The solution covers:

1. Creating and using ConfigMaps to configure pods.
2. Managing Secrets and using them in pods.
3. Debugging pod status issues related to Secrets.
4. Creating multi-container pods and initContainers.
5. Setting up Persistent Volumes (PVs) and Persistent Volume Claims (PVCs).
6. Mounting PVCs to pods for persistent storage.

---

## DevOps Assignment 3.1: Kubernetes Theory and Practical Tasks

**File:** `DevOps-Assignment-3.1-solu.md`

This task combines theoretical and practical Kubernetes knowledge. It includes:

### Theory Questions

1. Explaining Kubernetes architecture components (master, node, etcd, kube-apiserver).
2. Understanding pods and their differences from Docker containers.
3. Discussing Deployments, Services, and their types (ClusterIP, NodePort, LoadBalancer).
4. Exploring scaling and Horizontal Pod Autoscaler (HPA).

### Practical Tasks

1. Creating a Deployment with 3 replicas and exposing it via a ClusterIP service.
2. Testing load balancing and exposing the deployment using a NodePort service.
3. Setting up HPA to automatically scale pods based on CPU utilization.

---

## Prerequisites

- A working Kubernetes cluster (I usining Minikube cluster).
- `Minikube` installed and configured.
- Basic understanding of Kubernetes concepts.

---

## Notes

- Ensure all YAML files are properly indented and validated before applying them to your cluster.
- Use `minikube kubectl -- describe` and `minikube kubectl -- logs` commands to debug any issues.
- For persistent storage tasks, ensure your cluster supports the required storage class and access modes.

---
