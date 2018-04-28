# Understand Netoworking in _Pi_

In [part 1](../Quickstart/launching_your_first_pod.md) we looked at starting, inspecting and interacting with a pod. In this section we’ll look at the network layer.

![](https://trello-attachments.s3.amazonaws.com/5700ea0da7030dcf7485ed70/5a97dd05ecefd109f1b7e367/b6301e738fa97a06fad8140a2697c595/2.png)

### Private Network

When you signed up _Pi_, you will receive a default layer-2 virtual private network (in each region). Different accounts' networks are isolated from each other, and the Internet. A network has a virtual router, whose IP address may vary over time. The virtual router is used for outgoing traffic, but does not accept incoming requests.

> **Note**:
> The [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) of the network is set to `10.244.0.0/16`. We reserve a few addresses of your private network for the platform's own usage.

### Pod

When you launch a pod, it will be automatically placed in your private network. Each pod will receive a private IP address (aka _Pod IP_), which is static during the pod lifespan and is only returned when the pod is terminated (There is currently no way to control the IP allocation). _Pod IP_ is shared by all containers in a pod, as long as there are no port conflicts among containers within a pod.

Pods within the same network are reachable to each other (using _Pod IP_), but isolated from other networks (hence customers). Pods come with the access to the Internet by default, but they are not accessible from the Internet. This makes your application more secure as you can't accidently expose an important part of your infrastructure to the Internet.

### Service and Floating IP

_Service_ serves as load balancer in front of a fleet of pods. Each service is created with a private IP (_Service IP_) and an internal DNS name (_Service DNS Name_), which are accessible by other pods within the same network. In this case, the _service_ sits between the frontend and backend pods as internal LB.

To expose a _service_ on the Internet, a _Floating IP_ must be associated with the _service_:

```bash
$ pi create fip
104.154.140.179
```

Next define the service spec. Here we use the `LoadBalancer` type with the allocated floating IP, and use `selector` for pods with the label `app: nginx`, finally map the port to `80`:

```sh
$ cat <<EOF > /tmp/service.yml
apiVersion: v1
kind: Service
metadata:
  name: test-loadbalancer
spec:
  type: LoadBalancer
  loadBalancerIP: 104.154.140.179
  selector:
    app: nginx
  ports:
    - port: 8080
      targetPort: 80
EOF
```

Then let's create the backend pod with the label `app:nginx`. Here we use Nginx image, which listens to port `80`:
```sh
$ cat <<EOF > /tmp/nginx.yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx
EOF
```

OK, let's create:


```sh
$ pi create -f /tmp/service.yml
service "test-loadbalancer" created
$ pi create -f /tmp/nginx.yml
pod "nginx" created
```

Now you can open a browser and navigate to http://104.154.140.179:8080 where you’ll see Nginx home screen.

> ***Note***
> - You cannot attach multiple floating IPs to a _service_, but you can associate multiple services with one single floating IP (as long as no port flicts among different services).
> - Currently _service_ does not handle SSL termination. Please configure the backend pods to process the SSL requests.

Floating IPs are independent from the pod, therefore you need to release them separately:

```sh
$ pi delete pod nginx
pod "nginx" deleted
$ pi delete service test-loadbalancer
service "test-loadbalancer" deleted
$ pi delete fip 104.154.140.179
fip "104.154.140.179" deleted
```

> ***A note on FIP billing***:
> Resources in _Pi_ are billed per-second, apart from FIPs which cost $1 per month from the point of allocation. This is to prevent abuse. To stop you accidentally spending too much money on FIPs the best practice is to always release at the end of month.

In [the next chapter](../Quickstart/use_volume_for_stateful_workload.md) we'll explore how to use volumes for persistent data.
