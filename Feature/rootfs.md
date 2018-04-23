# Rootfs

![](https://trello-attachments.s3.amazonaws.com/5700ea0da7030dcf7485ed70/5a9941cae486d3ab50425859/d0a07eb7cdeaabc3350889bc176d705f/5.png)

A _roofs_ provides ephemeral storage for your workload. It is ideal for content like buffers, caches, scratch data, and other temporary data.

_rootfs_ is associated with container, instead of pod. Everyone container comes with its own dedicate rootfs. If a pod has only one container, there is only one rootfs in that pod; if a pod has four containers, there will be four rootfs in the pod.

_rootfs_ use the `EXT4` filesystem. The size is fixed: 10GB. 

Lifecycle
-----------------------

There is no way to control rootfs, or view it in the web console. However, the following are some principles:

- _rootfs_ is created and mounted automatically to the pod. You don't need to specify _rootofs_ when you launch pods.
- _rootfs_ is always mounted at the root path `/` in the container. There is no way to change that.
- You can't detach a rootfs from one pod and attach it to a different pod.
- The data in rootfs persists only during the lifetime of its associated pod. If the pod is restarted, data in the rootfs is reset.
- Currently there is no way to commit changes in a rootfs to a new Docker image.

Billing
-----------------------
- A pod may have one or more containers.
- Every container come with a default 10GB rootfs, e.g. if a pod have 3 containers, the pod has three rootfs, 30GB in total.
- Rootfs exits along with the lifecycle of the pod. Billing begins when the pod is created, ends when the pod terminates or exists.
- The price is $0.0000000386/GB/second, and a default duration of 60 seconds is applied.
