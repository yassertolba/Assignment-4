# DevOps Assignment 3.1

**Trainee Name: Yasser Ahmed**  
**Group: ALX2_SWD1_G1**  

## Table of Contents

## **Theory Questions (10 points)**

**a) Kubernetes Architecture**  
**b) Deployments and Services**  
**c) Scaling and Autoscaling**  

## a) Kubernetes Architecture

### • What are the core components of a Kubernetes cluster (e.g., master, node, etcd, kube-apiserver)? Briefly explain their roles

A Kubernetes cluster commonly contains one or more master nodes and one or more worker nodes. Every Kubernetes cluster must have at least one worker node.

![Kubernetes architecture](kubernetes-architecture.png)  
****Figure 2-1.*** Kubernetes architecture*

Figure 1 shows a typical Kubernetes cluster, highlighting three main components: the master node (also known as the control plane), the worker node, and the cloud API provider.

The master node is where all cluster management and execution decisions reside; it can be thought of as the mind of the Kubernetes cluster.

The worker node is where the pods are created and run, and a pod is the Kubernetes component inside which your application runs.

The master node is responsible for managing the worker nodes.

## Master Node (Control Plane)

The kube-apiserver component is the key component of the control plane or master node and is responsible for all the interactions within and outside of it.

The Kubernetes cluster runs on top of either a physical or virtual environment.

![Master node components](master-node-components.png)





















#### • What is a pod in Kubernetes, and how does it differ from a Docker container?

