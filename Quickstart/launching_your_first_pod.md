# Launch your first pod

First we’ll show you how to launch your first pod with a single container and interact with it. Let’s start with a simple busybox image.

```sh
$ cat <<EOF > /tmp/alpine.yml
apiVersion: v1
kind: Pod
metadata:
  name: alpine
spec:
  containers:
  - name: alpine
    image: alpine
EOF

$ pi create -f /tmp/alpine.yml
pod/alpine
```

> **A note on pod size**
With Pi, a pod receives a certain amount computational resources (CPU, RAM). The size of a pod is automatically calculated based on the pod spec, and applied to the pod when it is started. The default size for a pod is `S4` as you can. You can see what the various sizes mean on the pricing page [here](../Overview/pricing.md).

## Inspecting the pod

Before we look at alpine itself, let’s take a look at how you can inspect your pod.

```sh
$ pi get pods alpine -o yaml
apiVersion: v1
kind: Pod
metadata:
  annotations:
    id: fba32f34a9b6716326445c7b39188d1856d5641fe86fb745bf5c18ba8e529124
    sh_hyper_instancetype: s4
    zone: gcp-us-central1-c
  creationTimestamp: 2018-04-28T03:27:48Z
  name: alpine
  namespace: default
  uid: 201c9851-4a94-11e8-a873-42010a7f0002
spec:
  containers:
  - image: alpine
    name: alpine
    resources: {}
  nodeName: gcp-us-central1
status:
  conditions:
  - lastProbeTime: null
    lastTransitionTime: 2018-04-28T03:27:48Z
    status: "True"
    type: Initialized
  - lastProbeTime: null
    lastTransitionTime: 2018-04-28T03:27:52Z
    status: "True"
    type: Ready
  - lastProbeTime: null
    lastTransitionTime: 2018-04-28T03:27:48Z
    status: "True"
    type: PodScheduled
  containerStatuses:
  - containerID: hyper://e406ce32bd88321677cdb3c52f3c21329f0b4c47bd38ee17c709ffcbe1759a62
    image: sha256:3fd9065eaf02feaf94d68376da52541925650b81698c53c6824d92ff63f98353
    imageID: sha256:3fd9065eaf02feaf94d68376da52541925650b81698c53c6824d92ff63f98353
    lastState: {}
    name: alpine
    ready: true
    restartCount: 0
    state:
      running:
        startedAt: 2018-04-28T03:27:52Z
  phase: Running
  podIP: 10.244.106.62
  qosClass: Burstable
  startTime: 2018-04-28T03:27:48Z
```

It shows you information about your pod, most of which is outside the scope of this section. You can see `sh_hyper_instancetype` however, which is useful for in case you cannot remember which size you started your pods as.

## Interacting with the pod

We can use `pi exec` to run commands inside our pod. Let's login the busybo!

```sh
$ pi exec alpine -c alpine -- cat /etc/issue
Welcome to Alpine Linux 3.7
Kernel \r on an \m (\l)
```

## Cleaning up

One of the key benefits of _Pi_ is reducing your operations costs. To make sure that you’re not being billing for any resources that you’re not using, you should know how to clean up all resources once you are finished with them.

```sh
$ pi delete pod alpine
pod "alpine" deleted
```

## How much did that cost?

In Pi, all resources are billed per-second (apart from _Service_). The billing for the example above would break down as follows.

**S4 size pod:** $0.000009/second, ~$7.76/month (with automatic sustained discount)

**10GB rootfs on s4 pod:** $0.0000000386/GB/second, $1/month

So the total monthly price for running a pod at s4 size is **$8.76**. You can always check your current usage costs in the [Hyper console](https://console.hyper.sh/billing/credit).

>**Note on pricing**
>Remember that Hyper allows you to choose the size of your pod at start time so the billing information above applies to this example only. Check the pricing page for more information: [https://hyper.sh/pricing.html](https://hyper.sh/pricing.html)

That's it for part 1. In [the next chapter](../Quickstart/understand_network.md) we'll learn about how network works in _Pi_.
