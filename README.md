# kubectl-nsenter

_Simple kubectl plugin to take pod name, SSH onto node and spawn an nsenter shell._

**:heavy_exclamation_mark: Since we use `crictl`, this plugin may only work with `containerd` and not with straight Docker ..**

So, I made a quick plugin for kubectl that allows to enter the network namespace of the pod to run things like netstat, tcpdump, dig, _etc pp_

The node (if it is a full-blown OS like Ubuntu 18.04) has all the debugging
tools needed, and so `FROM scratch` containers or containers runnning as
non-root can be debugged from the host using `nsenter` to join the network namespace or
the PID namespace of the container.

1. Retrieve node name
2. SSH onto node
3. Use `crictl` to obtain pod ID
4. Use `crictl` to obtain container ID
5. Use `crictl` to obtain target PID
6. Use `nsenter` to join process namespace
7. Spawn bash as debug shell

## Installation

Download, make executable, and move to PATH.

## Usage

    $ kubectl nsenter --help
    SSH into Kubernetes nodes and nsenter into network namespace of a pod

    Example:

      kubectl nsenter -n goe-dev api-dev-6d94fd5c98-xqdv5 -c php

    Options:

      -n, --namespace='': Namespace of the pod
      -c, --container='': Name of container of a pod
