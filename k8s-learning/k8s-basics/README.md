From https://kubernetes.io/docs/tutorials/kubernetes-basics/

Note that I had to install `minikube` (https://minikube.sigs.k8s.io/docs/start/) to get started:
- After installation I ran into this issue:

  ```
  �  Unable to pick a default driver. Here is what was considered, in preference order:
  ▪ podman: Not healthy: "sudo -n -k podman version --format {{.Version}}" exit status 1: sudo: a password is required
  ▪ podman: Suggestion: Add your user to the 'sudoers' file: 'stinker ALL=(ALL) NOPASSWD: /usr/bin/podman' , or run 'minikube config set rootless true' <https://podman.io>
  ```

  Setting it to run rootless would throw up another error: `Exiting due to MK_USAGE:
  --container-runtime must be set to "containerd" or "cri-o" for rootless`, so I decided to create a
  file `/etc/sudoers.d/podman` (all folders in that directory are sourced by the sudoers file)
- I still had issues with podman - "OCI runtime error: crun: chmod `run/shm`: Operation not
  supported", and then
  ```
  �  Restarting existing podman container for "minikube" ...
  �  Failed to start podman container. Running "minikube delete" may fix it: driver start: start: sudo  -n podman start --cgroup-manager cgroupfs minikube: exit status 125
  ```

  I'm going to try updating my system/podman/crun (???) because apparently it's an issue that's
  caused by a change in the Linux kernel, and was subsequently fixed. See
  https://github.com/containers/crun/issues/1308
    - It worked! I was on version 1.9.1 and `sudo yum update` updated `crun` to 1.9.2, which
      included the fix
- Installing `minikube` also installs Kubernetes. It doesn't install `kubectl`, but you can use it
  via minikube (which will install it locally if it doesn't already exist). By creating an alias,
  `alias kubectl="minikube kubectl --"`, it will basically function identically on the command line
  to `kubectl`, though it will only talk to the cluster that minikube creates.
