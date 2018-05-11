# Pod

> _Pods_ are the smallest deployable units of computing that can be created and managed in Pi.

What is a Pod?
-------------------------

A _pod_ (as in a pod of whales or pea pod) is a group of one or more containers (such as Docker containers), with shared storage/network, and a specification for how to run the containers.  A pod's contents are always co-located and co-scheduled, and run in a shared context.  A pod models an application-specific "logical host" - it contains one or more application containers which are relatively tightly coupled &mdash; in a pre-container world, they would have executed on the same physical or virtual machine.

![Pod Overview](https://trello-attachments.s3.amazonaws.com/5700ea0da7030dcf7485ed70/5a8ea6c5a9972aaaaa7fec5c/d4de3e6c38d156e393eac25f09919b5a/1.png)

Containers within a pod share an IP address and port space, and can find each other via `localhost`. They can also communicate with each other using standard inter-process communications like SystemV semaphores or POSIX shared memory.  Containers in different pods have distinct IP addresses and cannot communicate by IPC. These containers usually communicate with each other via Pod IP addresses.

Applications within a pod also have access to shared volumes, which are defined as part of a pod and are made available to be mounted into each application's filesystem.

Like individual application containers, pods are considered to be relatively ephemeral (rather than durable) entities. Pods are created, assigned a unique ID (UID) and name. The pod name remains until termination (according to restart policy) or deletion. In the case of pod failure, the pod can be replaced by an identical pod, with even the same name, but with a new UID.

When something is said to have the same lifetime as a pod, such as a _[rootfs](../Feature/rootfs.md)_, that means that it exists as long as that pod (with that UID) exists. If that pod is deleted for any reason, even if an identical replacement is created, the related thing (e.g. volume) is also destroyed and created anew.

_Pod_ is constrained to an availability zone. There is no way to migrate a pod to a different zone except to recreate it. Pod name is regional, e.g. no two pods in the same region can have the same pod name.

Pod Spec
-------------------------

_Pi_ uses the standard Kubernetes [Pod Spec](../Reference/API/v1.9/pod/index.md#pod-1):

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.7.9
    ports:
    - containerPort: 80
```

The API accepts a JSON string for _Pod Spec_.

Pod Size
-------------------------

A pod receives a certain amount of computational resources (CPU, RAM), which are shared by all application containers within the pod. We provide a set of resource configurations (aka _Pod Size_)for you to choose when launching a new pod. Different sizes come with difference prices (See [our pricing page](../Overview/pricing.md) for more details).

The size of a pod is automatically calculated based on the pod spec, and applied to the pod when it is started:

- Calculate the sum of `spec.containers[].resources.limits.memory` field of all containers in the pod spec, and ignore the rest, e.g.:
  - `spec.containers[].resources.limits.cpu`  
  - `spec.containers[].resources.requests.cpu`
  - `spec.containers[].resources.requests.memory`
- If a container has no `spec.containers[].resources.limits.memory` field, it is counted as `0`.
- The pod size will be the nearest size that has no less memory than the sum value, e.g. `M1 (1GB)` in the case of 912MB
- If no memory limits are specified in the spec, the default pod size is `S4`.
- Currently, the largest pod size we support is `L2 (16GB)`. If your pod requires more memory than this, the API request will be rejected.

 For example:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multiple-nginx-instancetype
spec:
  containers:
  - name: nginx1
    image: nginx
  - name: nginx2
    image: nginx
    resources:
        limits:
            cpu: 0
            memory: 0
  - name: nginx3
    image: nginx
    resources:
        limits:
            cpu: 0.5
            memory: 0.5Mi
  - name: nginx4
    image: nginx
    resources:
        limits:
            cpu: 100m
            memory: 1Mi
```

In the above spec, four containers are present:
- _nginx1_ has no memory limits, then it is counted as `0`.
- The memory limit of _nginx2_ is `0`, it is counted as `0`.
- The memory limit of _nginx3_ is `0.5Mi`, which is `0.5GB`.
- The memory limit of _nginx4_ is `1Mi`, which is `1GB`.

The total memory limits is: `0 + 0 + 0.5 + 1 = 1.5GB`, then the pod size will be `M2 (2GB)`.

Termination of Pods
-------------------------

Because pods represent running processes, it is important to allow those processes to gracefully terminate when they are no longer needed (vs being violently killed with a KILL signal and having no chance to clean up). Users should be able to request deletion and know when processes terminate, but also be able to ensure that deletes eventually complete. When a user requests deletion of a pod the system records the intended grace period before the pod is allowed to be forcefully killed, and a TERM signal is sent to the main process in each container. Once the grace period has expired the KILL signal is sent to those processes and the pod is then deleted from the API server. If the container manager is restarted while waiting for processes to terminate, the termination will be retried with the full grace period.

An example flow:

1. User sends API request to delete Pod, with default grace period (30s)
2. The Pod is updated with the time beyond which the Pod is considered "dead" along with the grace period.
3. Pod shows up as "Terminating" when listed in client commands
4. The processes in the Pod are sent the TERM signal.
5. When the grace period expires, any processes still running in the Pod are killed with SIGKILL.
6. _Pi_ will finish deleting the Pod on the API server by setting grace period 0 (immediate deletion). The Pod disappears from the API and is no longer visible from the client.

By default, all deletes are graceful within 30 seconds. User may override the default and specify their own value. The value `0` [force deletes](../Feature/pod.md#force-deletion-of-pods) the pod.

Force deletion of pods
-------------------------

Force deletion of a pod is defined as deletion of a pod immediately. When a force deletion is performed, _Pi_ does not wait for confirmation that the pod has been terminated on the node it was running on. It removes the pod in the API immediately so a new pod can be created with the same name. Pods that are set to terminate immediately will still be given a small grace period before being force killed.

Force deletions can be potentially dangerous for some pods and should be performed with caution.

Pod Limits
-------------------------

- Max open files: 1000000:1000000
- Max processes: 30604:30604
- Max pending signals: 30604:30604

> NOTE: These values can be overwritten by setting `rlimit` in the pod.
