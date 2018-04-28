What is Pi?
-----------------------------

> If you have questions about whether _Pi_ is right for you, [contact Hyper Sales](mailto:contact@hyper.sh). If you have technical questions about Pi, use the [our forum](https://forum.hyper.sh), or contact us on [Twitter](https://twitter.com/hyper_sh).

_Pi_ is a **Serverless Container Platform**. It is the simplest and fastest way to run containers in the cloud, bypassing all those infrastructure headaches, to let companies of all sizes embrace the value of containers immediately.

_Pi_ is "Serverless" because there is no VM node, nor cluster in the system. To run containers, the only thing to do is to call Pi' RESTful API.

You might compare _Pi_ with [Function-as-a-Service](https://en.wikipedia.org/wiki/Function_as_a_service), with the difference that it runs Docker image, not function.

### _Pi_ vs. FaaS
<table class="table table-bordered table-striped table-condensed">
<tr>
<td></td><td>Pi</td><td>FaaS</td>
</tr>
<tr>
<td>Artifact</td><td>Docker Image</td><td>Source Code</td>
</tr>
<tr>
<td>Supported Language</td><td>Anything (Code, Binary, Shell Script, etc.)</td><td>Limited</td>
</tr>
<td>API</td><td>Kubernetes Pod API</td><td>AWS Specific</td>
</tr>
<td>Execution Instance</td><td>Container (Pod)</td><td>Function Call</td>
</tr>
</table>



Benefits
-----------------------------

_Pi_ gets out of the way where it matters, by making the processes of running containers as simple and fast as possible, so that companies can _run as many containers as as they want, at anytime, without preparing anything_.

- **INSTANT PROVISIONING** - In most cases, _Pi_ takes only **3 seconds** to provision new containers from scratch. It is by far the fastest in the market.

- **NO CLUSTER TO MANAGE** - With Pi, you no longer need to provision, configure, and scale clusters of virtual machines to run containers. It lets you **focus on running workloads, no distraction of servers, nor distraction of clusters.**.

- **JUST-IN-TIME SCALING** - _Pi_ allows applications to scale with **JIT** manner, thus eliminates the need to prepare extra capacity for peak.

- **COST EFFICIENT** - _Pi_ uses a **Per Second** pricing model. Spin up 100 containers in 3 seconds, run 30s, shutdown in 1s, and only pay for 3+30+1=33s.

- **PERSISTENT DATA** – Run stateful workloads with Pi's persistent volume. Multiple replicas will be automatically created with each volume to protect your data from failures.

- **PRIVATE NETWORK Network (FREE)** - _Pi_ creates default private network for each user. Your container traffic is safely isolated from the rest. And the best part is that all traffic is FREE!

Features
----------------------

_Pi_ provides the following features:

- [**Pod**](../Feature/pod.md) - atomic group of one or more containers.

- [**Rootfs**](../Feature/rootfs.md) - temporary disk space for containers in the pod, available along with the pod lifecycle.

- [**Volume**](../Feature/volume.md) - persistent storage for stateful workload, automatically replicated to protect your data from failures.

- [**Network**](../Feature/network.md) - Layer-2 private network dedicated to your account to isolate your traffic from the rest customers.

- [**Service**](../Feature/service.md) - Kubernetes abstraction to provide a network connection to one or more pods. For more detail see the background on [how services work](http://kubernetes.io/docs/user-guide/services).

- [**Floating IP**](../Feature/fip.md) - Static public IPv4 address to associate with your pod, making them accessible on the Internet.

- [**Secret**](../Feature/secret.md) - Encrypted object that contains a small amount of sensitive data such as a password, a token, or a key.

- [**Region**](../Feature/region.md) - Separate geographic physical data centers, completely indepedent from one another.

- [**Availability Zones**](../Feature/region.md) - Isolated physical locations within a region, interconnected through low-latency links.
