# Hello Minikube

Source: https://kubernetes.io/docs/tutorials/hello-minikube/

This is basically just a bunch of commands you run in a row.

After running `minikube start`, in a separate terminal run `minikube dashboard --url` to get a link
to a web dashboard.
- The tutorial assumes you're running this locally. Since I'm running minikube on a local NAS, I
  need to forward the ports from kubernetes, as per https://stackoverflow.com/a/53830578 (and also
  open port 8001 on my server's firewall): `kubectl proxy --address='0.0.0.0' --disable-filter=true`

  Note that running this means that everything gets proxied through 8001 (??) and fucks up later K8s
  actions, including pulling images. So if you get `container "agnhost" in pod "hello-node-*" is
  waiting to start: trying and failing to pull image`, just kill the proxy command, re-run the
  `hello-node` deployment creation, and then re-run the proxy command.

## Creating a deployment
A pod is a group of containers that are treated as one unit for administration and networking.

A deployment monitors the health of pods and restarts their container(s) if they terminate or
otherwise become unhealthy. "Deployments are the recommended way to manage the creation and scaling
of Pods", which implies there's other way to manage them?

- `kubectl create` to create a Deployment that manages a Pod. (This capitalisation is straight out
   of the tutorial, I'm not that annoying).

   ```
   # Run a test container image that includes a webserver
   kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- /agnhost netexec --http-port=8080
   ```
- Getting information about the deployment and pod:
  - `kubectl get deployments` - list of deployments
  - `kubectl get pods` - list of pods
  - `kubectl get events` - this is for what Kubectl and the pods themselves are doing. Like,
    meta-logs about the system.
  - `kubectl config view` - kubectl configuration, outputs (by default) in YAML format
  - `kubectl logs [pod name]` - application logs for a container in a pod

## Creating a service
By default, the pod is only accessible by its internal IP address within the Kubernetes cluster
(i.e. within the running containers and stuff, so not accessible even by the host??).

To access containers from outside the Kubernetes virtual network, you have to expose the pod as a
Kubernetes service.

We can do this with `kubectl expose`. In this particular case:
```
kubectl expose deployment hello-node --type=LoadBalancer --port=8080
```

The `--type=LoadBalancer` option "indicates that you want to expose your service outside of the
cluster". I thought that was the whole purpose of exposing a pod as a service?
- The other options for `--type` are `ClusterIP` and `NodePort`, with the default being `ClusterIP`.
  No explanation on the `kubectl` docs as to how these options differ from each other.

`kubectl get services` to view the created services. Usually, the `LoadBalancer` type will get an
external IP address ("usually" means "when running on a cloud provider"); on `minikube`, it just
means you can access the service via `minikube service`.

`minikube service hello-node` (or in our case `minikube service hello-node --url=true`) to get a
link to see the app.
- Well I need to figure out how to fucking get this working now, great
- I can't figure it out. Oh well. I've opened a bunch of different ports on both the regular network
  interface of the NAS as well as the `podman1` and `veth0` interfaces, and created a custom route
  that sets the IP address of my NAS as the gateway for the `192.168.49.1/24` IP range that
  `podman1` provides (?). Still can't route to it from my main PC.
- just curl it lmao
  - Yeah that works

## Enable addons
Minikube's got a bunch of neato addons I guess, such as the `metrics-server`. This can be enabled
with `minikube addons enable metrics-server`.

## Clean up
Deleting the cluster resources:
```
kubectl delete service hello-node
kubectl delete deployment hello-node
```

Stop the minikube cluster with `minikube stop`, and *optionally* delete the VM with `minikube
delete`. Keep it around if you don't want to wait for everything to spin up again.
