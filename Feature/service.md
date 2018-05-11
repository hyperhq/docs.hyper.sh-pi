# Service

A `Service` is an abstraction which defines a logical set of `Pods` and a policy by which to access them - sometimes called a micro-service. The set of `Pods` targeted by a `Service` is (usually) determined by a [`Label Selector`](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#label-selectors) (see below for why you might want a `Service` without a selector).

As an example, consider an image-processing backend which is running with 3 replicas.  Those replicas are fungible - frontends do not care which backend they use.  While the actual `Pods` that compose the backend set may change, the frontend clients should not need to be aware of that or keep track of the list of backends themselves.  The `Service` abstraction enables this decoupling.

_Service_ is regional. You can associate pods in all availibity zones of the same region to the service.

## Defining a service

A `Service` in Kubernetes is a REST object, similar to a `Pod`.  Like all of the REST objects, a `Service` definition can be POSTed to the apiserver to create a new instance.  For example, suppose you have a set of `Pods` that each expose port 9376 and carry a label `"app=MyApp"`.

```yaml
kind: Service
apiVersion: v1
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
  - protocol: TCP
    port: 80
    targetPort: 9376
```

This specification will create a new `Service` object named "my-service" which targets TCP port 9376 on any `Pod` with the `"app=MyApp"` label.  This `Service` will also be assigned an IP address (sometimes called the "cluster IP"), which is used by the service proxies (see below).  The `Service`'s selector will be evaluated continuously and the results will be POSTed to an `Endpoints` object also named "my-service".

Note that a `Service` can map an incoming port to any `targetPort`.  By default the `targetPort` will be set to the same value as the `port` field.  Perhaps more interesting is that `targetPort` can be a string, referring to the name of a port in the backend `Pods`.  The actual port number assigned to that name can be different in each backend `Pod`. This offers a lot of flexibility for
deploying and evolving your `Services`. For example, you can change the port number that pods expose in the next version of your backend software, without breaking clients.

`Services` support `TCP` and `UDP` for protocols. The default is `TCP`.

## Multi-Port Services

Many `Services` need to expose more than one port.  For this case, we support multiple port definitions on a `Service` object.  When using multiple ports you must give all of your ports names, so that endpoints can be disambiguated.  For example:

```yaml
kind: Service
apiVersion: v1
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
  - name: http
    protocol: TCP
    port: 80
    targetPort: 9376
  - name: https
    protocol: TCP
    port: 443
    targetPort: 9377
```

## Publishing services - service types

For some parts of your application (e.g. frontends) you may want to expose a Service onto a [_Floating IP_](../Feature/fip.md) address.

`ServiceTypes` allow you to specify what kind of service you want. The default is `ClusterIP`.

`Type` values and their behaviors are:

   * `ClusterIP`: Exposes the service on an interal IP. Choosing this value makes the service only reachable from within the internal network. This is the default `ServiceType`.
   * `LoadBalancer`: Exposes the service externally using a _floating IP_.

### Type LoadBalancer

To associate a _service_ with a _floating IP_, the service type must be `LoadBalancer`. In this case, the service acts as a public-facing load balancer in front of the pod fleets behind the service. For example:

```yaml
kind: Service
apiVersion: v1
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
  - protocol: TCP
    port: 80
    targetPort: 9376
  loadBalancerIP: 78.11.24.19
  type: LoadBalancer
```
