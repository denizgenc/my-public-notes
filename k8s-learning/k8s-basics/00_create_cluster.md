# Using Minikube to Create a Cluster

Source: https://kubernetes.io/docs/tutorials/kubernetes-basics/create-cluster/cluster-intro/

# What's a cluster?
Basically, a group of computers that can run containers. The point of Kubernetes is that it manages
assigning containers to the computers for you, more efficiently than can be done manually.

There's two types of resources in a Kubernetes cluster:
- The Control Plane
  - This is the central resource which manages the cluster. Scheduling, maintaining applications'
    desired state, scaling applications, rolling out updates.
- Nodes
  - VMs or physical computers. These will have a Kubelet, which manages the node and talks to the
    Control Plane; and a container runtime - much like Docker or Podman but usually more
    lightweight, such as `containerd` or CRI-O.

Production clusters should have at least 3 nodes to maintain redundancy.

The Kubernetes API allows both the kubelets on the nodes and end users to talk to the
cluster/control plane.

Minikube creates a VM on your local machine and starts a cluster with one node.
