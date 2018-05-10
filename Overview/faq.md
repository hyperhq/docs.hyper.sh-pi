FAQ
-------------

### How fast is _Pi_ to launch a pod?
> It usually takes **3 seconds** to launch a new pod in Pi. However, there are several factors that could affect the performance, e.g. the number of containers in the pod, image size, application startup speed, etc.

### How to schedule pods in _Pi_ ?
> No, you don't need to. _Pi_ is a serverless container platform, which means that you can just run containers without worrying about the underlying infrastructure. Things like capacity planning, scheduling , resource utilization and cluster autoscaling are all gone.

### How many pods can I run in Pi?
> By default, you are limited to running up to 100 pods per region, though you may [request to increase the quota](./quota_and_limits.md).

### How does networking work in Pi?
> By default, _Pi_ creates a private network dedicated for each account, so as to separate different customers:
>
> - When you create a new pod, it is placed in your own network.
> - Every pod receives a private IP address (specific to the network it resides in). The IP address is attached exclusively with the pod and is only returned when the pod is terminated.
> - A pod is accessible in the network where it resides (using its private IP address), but no other networks.
> - All pods come with the access to the Internet, however none is by default accessible from the Internet. This is adequate for most non-public-facing pods. For public-facing pods, [_Service_](../Feature/service.md) and [_Floating IP_](../Feature/fip.md) are demanded.
> - Service serves as load balancer in front of backend pods. For more detail see the background on [how services work](http://kubernetes.io/docs/user-guide/services).

### How to access a pod in Pi?
Use [`pi exec`](../Reference/CLI/pod/exec.md) command:

```sh
$ pi exec alpine --container alpine -- cat /etc/issue
Welcome to Alpine Linux 3.7
Kernel \r on an \m (\l)
```

### What happens to my data when a pod terminates?
> - The data stored on the pod's `rootfs` will persist only as long as that pod exists.
> - The data that is stored on additional volumes will persist independently of pods.

### Which datacenter is _Pi_ using?
Similar with other platforms like [_Heroku_](https://heroku.com): _Pi_ does not run in our own datacenter, it runs on [Google Cloud](https://cloud.google.com/). Therefore, the region and zone naming in _Pi_ follows the pattern of GCP:

<table class="table table-bordered table-striped table-condensed">
<tr>
<td>Cloud</td><td>Region</td><td>Location</td><td>Zones</td>
</tr>
<tr>
<td>Google Cloud</td><td>gcp-us-central1</td><td>Iowa</td><td>gcp-us-central1-a,gcp-us-central1-c</td>
</tr>
</table>

### Do your prices include taxes?
> Except as otherwise noted, our prices are exclusive of applicable taxes and duties, including VAT and applicable sales tax.
