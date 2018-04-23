# Service

A `Service` is an abstraction which defines a logical set of `Pods` and a policy by which to access them - sometimes called a micro-service. The set of `Pods` targeted by a `Service` is (usually) determined by a [`Label Selector`](/docs/concepts/overview/working-with-objects/labels/#label-selectors) (see below for why you might want a `Service` without a selector).

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

## Discovering services

We support 2 primary modes of finding a `Service` - environment variables and DNS.

### Environment variables

When a `Pod` is run on a `Node`, the kubelet adds a set of environment variables
for each active `Service`.  It supports both [Docker links
compatible](https://docs.docker.com/userguide/dockerlinks/) variables (see
[makeLinkVariables](http://releases.k8s.io/{{page.githubbranch}}/pkg/kubelet/envvars/envvars.go#L49))
and simpler `{SVCNAME}_SERVICE_HOST` and `{SVCNAME}_SERVICE_PORT` variables,
where the Service name is upper-cased and dashes are converted to underscores.

For example, the Service `"redis-master"` which exposes TCP port 6379 and has been
allocated cluster IP address 10.0.0.11 produces the following environment
variables:

```sh
REDIS_MASTER_SERVICE_PORT=6379
REDIS_MASTER_SERVICE_HOST=10.0.0.11
REDIS_MASTER_PORT=tcp://10.0.0.11:6379
REDIS_MASTER_PORT_6379_TCP=tcp://10.0.0.11:6379
REDIS_MASTER_PORT_6379_TCP_PROTO=tcp
REDIS_MASTER_PORT_6379_TCP_PORT=6379
REDIS_MASTER_PORT_6379_TCP_ADDR=10.0.0.11
```

*This does imply an ordering requirement* - any `Service` that a `Pod` wants to
access must be created before the `Pod` itself, or else the environment
variables will not be populated.  DNS does not have this restriction.

### DNS

An option (though strongly recommended) is a DNS server.  The DNS server watches the Kubernetes API for new `Services` and creates a set of DNS records for each.  If DNS has been enabled throughout the cluster then all `Pods` should be able to do name resolution of `Services` automatically.

For example, if you have a `Service` called `"my-service"` in Kubernetes `Namespace` `"my-ns"` a DNS record for `"my-service.my-ns"` is created. `Pods` which exist in the `"my-ns"` `Namespace` should be able to find it by simply doing
a name lookup for `"my-service"`. `Pods` which exist in other `Namespaces` must qualify the name as `"my-service.my-ns"`.  The result of these name lookups is the cluster IP.

Kubernetes also supports DNS SRV (service) records for named ports.  If the `"my-service.my-ns"` `Service` has a port named `"http"` with protocol `TCP`, you can do a DNS SRV query for `"_http._tcp.my-service.my-ns"` to discover the port
number for `"http"`.

The Kubernetes DNS server is the only way to access services of type `ExternalName`. More information is available in the [DNS Pods and Services](/docs/concepts/services-networking/dns-pod-service/).

## Publishing services - service types

For some parts of your application (e.g. frontends) you may want to expose a Service onto a [_Floating IP_](../Feature/fip.md) address.

`ServiceTypes` allow you to specify what kind of service you want. The default is `ClusterIP`.

`Type` values and their behaviors are:

   * `ClusterIP`: Exposes the service on a cluster-internal IP. Choosing this value
     makes the service only reachable from within the cluster. This is the
     default `ServiceType`.
   * `LoadBalancer`: Exposes the service externally using a cloud provider's load balancer.
     `NodePort` and `ClusterIP` services, to which the external load balancer will route,
     are automatically created.
   * `ExternalName`: Maps the service to the contents of the `externalName` field
     (e.g. `foo.bar.example.com`), by returning a `CNAME` record with its value.
     No proxying of any kind is set up. This requires version 1.7 or higher of
     `kube-dns`.

### Type LoadBalancer

On cloud providers which support external load balancers, setting the `type`
field to `"LoadBalancer"` will provision a load balancer for your `Service`.
The actual creation of the load balancer happens asynchronously, and
information about the provisioned balancer will be published in the `Service`'s
`status.loadBalancer` field.  For example:

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
  clusterIP: 10.0.171.239
  loadBalancerIP: 78.11.24.19
  type: LoadBalancer
status:
  loadBalancer:
    ingress:
    - ip: 146.148.47.155
```

Traffic from the external load balancer will be directed at the backend `Pods`,
though exactly how that works depends on the cloud provider. Some cloud providers allow
the `loadBalancerIP` to be specified. In those cases, the load-balancer will be created
with the user-specified `loadBalancerIP`. If the `loadBalancerIP` field is not specified,
an ephemeral IP will be assigned to the loadBalancer. If the `loadBalancerIP` is specified, but the
cloud provider does not support the feature, the field will be ignored.

**Special notes for Azure**: To use user-specified public type `loadBalancerIP`, a static type
public IP address resource needs to be created first, and it should be in the same resource
group of the cluster. Then you could specify the assigned IP address as `loadBalancerIP`.

#### Internal load balancer
In a mixed environment it is sometimes necessary to route traffic from services inside the same VPC.

In a split-horizon DNS environment you would need two services to be able to route both external and internal traffic to your endpoints.

This can be achieved by adding the following annotations to the service based on cloud provider.

{% capture default_tab %}
Select one of the tabs.
{% endcapture %}

{% capture gcp %}
```yaml
[...]
metadata:
    name: my-service
    annotations:
        cloud.google.com/load-balancer-type: "Internal"
[...]
```
Use `cloud.google.com/load-balancer-type: "internal"` for masters with version 1.7.0 to 1.7.3.
For more information, see the [docs](https://cloud.google.com/kubernetes-engine/docs/internal-load-balancing).
{% endcapture %}

{% capture aws %}
```yaml
[...]
metadata:
    name: my-service
    annotations:
        service.beta.kubernetes.io/aws-load-balancer-internal: 0.0.0.0/0
[...]
```
{% endcapture %}

{% capture azure %}
```yaml
[...]
metadata:
    name: my-service
    annotations:
        service.beta.kubernetes.io/azure-load-balancer-internal: "true"
[...]
```
{% endcapture %}

{% capture openstack %}
```yaml
[...]
metadata:
    name: my-service
    annotations:
        service.beta.kubernetes.io/openstack-internal-load-balancer: "true"
[...]
```
{% endcapture %}

{% assign tab_names = 'Default,GCP,AWS,Azure,OpenStack' | split: ',' | compact %}
{% assign tab_contents = site.emptyArray | push: default_tab | push: gcp | push: aws | push: azure | push: openstack %}
{% include tabs.md %}

#### SSL support on AWS
For partial SSL support on clusters running on AWS, starting with 1.3 three
annotations can be added to a `LoadBalancer` service:

```yaml
metadata:
  name: my-service
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-ssl-cert: arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012
```

The first specifies the ARN of the certificate to use. It can be either a
certificate from a third party issuer that was uploaded to IAM or one created
within AWS Certificate Manager.

```yaml
metadata:
  name: my-service
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-backend-protocol: (https|http|ssl|tcp)
```

The second annotation specifies which protocol a pod speaks. For HTTPS and
SSL, the ELB will expect the pod to authenticate itself over the encrypted
connection.

HTTP and HTTPS will select layer 7 proxying: the ELB will terminate
the connection with the user, parse headers and inject the `X-Forwarded-For`
header with the user's IP address (pods will only see the IP address of the
ELB at the other end of its connection) when forwarding requests.

TCP and SSL will select layer 4 proxying: the ELB will forward traffic without
modifying the headers.

In a mixed-use environment where some ports are secured and others are left unencrypted,
the following annotations may be used:

```yaml
    metadata:
      name: my-service
      annotations:
        service.beta.kubernetes.io/aws-load-balancer-backend-protocol: http
        service.beta.kubernetes.io/aws-load-balancer-ssl-ports: "443,8443"
```

In the above example, if the service contained three ports, `80`, `443`, and
`8443`, then `443` and `8443` would use the SSL certificate, but `80` would just
be proxied HTTP.

Beginning in 1.9, services can use [predefined AWS SSL policies](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-security-policy-table.html)
for any HTTPS or SSL listeners. To see which policies are available for use, run
the awscli command:

```bash
aws elb describe-load-balancer-policies --query 'PolicyDescriptions[].PolicyName'
```

Any one of those policies can then be specified using the
"`service.beta.kubernetes.io/aws-load-balancer-ssl-negotiation-policy`"
annotation, for example:

```yaml
    metadata:
      name: my-service
      annotations:
        service.beta.kubernetes.io/aws-load-balancer-ssl-negotiation-policy: "ELBSecurityPolicy-TLS-1-2-2017-01"
```

#### PROXY protocol support on AWS

To enable [PROXY protocol](https://www.haproxy.org/download/1.8/doc/proxy-protocol.txt)
support for clusters running on AWS, you can use the following service
annotation:

```yaml
    metadata:
      name: my-service
      annotations:
        service.beta.kubernetes.io/aws-load-balancer-proxy-protocol: "*"
```

Since version 1.3.0 the use of this annotation applies to all ports proxied by the ELB
and cannot be configured otherwise.

#### ELB Access Logs on AWS

There are several annotations to manage access logs for ELB services on AWS.

The annotation `service.beta.kubernetes.io/aws-load-balancer-access-log-enabled`
controls whether access logs are enabled.

The annotation `service.beta.kubernetes.io/aws-load-balancer-access-log-emit-interval`
controls the interval in minutes for publishing the access logs. You can specify
an interval of either 5 or 60.

The annotation `service.beta.kubernetes.io/aws-load-balancer-access-log-s3-bucket-name`
controls the name of the Amazon S3 bucket where load balancer access logs are
stored.

The annotation `service.beta.kubernetes.io/aws-load-balancer-access-log-s3-bucket-prefix`
specifies the logical hierarchy you created for your Amazon S3 bucket.

```yaml
    metadata:
      name: my-service
      annotations:
        service.beta.kubernetes.io/aws-load-balancer-access-log-enabled: "true"
        # Specifies whether access logs are enabled for the load balancer
        service.beta.kubernetes.io/aws-load-balancer-access-log-emit-interval: "60"
        # The interval for publishing the access logs. You can specify an interval of either 5 or 60 (minutes).
        service.beta.kubernetes.io/aws-load-balancer-access-log-s3-bucket-name: "my-bucket"
        # The name of the Amazon S3 bucket where the access logs are stored
        service.beta.kubernetes.io/aws-load-balancer-access-log-s3-bucket-prefix: "my-bucket-prefix/prod"
        # The logical hierarchy you created for your Amazon S3 bucket, for example `my-bucket-prefix/prod`
```

#### Connection Draining on AWS

Connection draining for Classic ELBs can be managed with the annotation
`service.beta.kubernetes.io/aws-load-balancer-connection-draining-enabled` set
to the value of `"true"`. The annotation
`service.beta.kubernetes.io/aws-load-balancer-connection-draining-timeout` can
also be used to set maximum time, in seconds, to keep the existing connections open before deregistering the instances.


```yaml
    metadata:
      name: my-service
      annotations:
        service.beta.kubernetes.io/aws-load-balancer-connection-draining-enabled: "true"
        service.beta.kubernetes.io/aws-load-balancer-connection-draining-timeout: "60"
```

#### Other ELB annotations

There are other annotations to manage Classic Elastic Load Balancers that are described below.

```yaml
    metadata:
      name: my-service
      annotations:
        service.beta.kubernetes.io/aws-load-balancer-connection-idle-timeout: "60"
        # The time, in seconds, that the connection is allowed to be idle (no data has been sent over the connection) before it is closed by the load balancer

        service.beta.kubernetes.io/aws-load-balancer-cross-zone-load-balancing-enabled: "true"
        # Specifies whether cross-zone load balancing is enabled for the load balancer

        service.beta.kubernetes.io/aws-load-balancer-additional-resource-tags: "environment=prod,owner=devops"
        # A comma-separated list of key-value pairs which will be recorded as
        # additional tags in the ELB.

        service.beta.kubernetes.io/aws-load-balancer-healthcheck-healthy-threshold: ""
        # The number of successive successful health checks required for a backend to
        # be considered healthy for traffic. Defaults to 2, must be between 2 and 10

        service.beta.kubernetes.io/aws-load-balancer-healthcheck-unhealthy-threshold: "3"
        # The number of unsuccessful health checks required for a backend to be
        # considered unhealthy for traffic. Defaults to 6, must be between 2 and 10

        service.beta.kubernetes.io/aws-load-balancer-healthcheck-interval: "20"
        # The approximate interval, in seconds, between health checks of an
        # individual instance. Defaults to 10, must be between 5 and 300
        service.beta.kubernetes.io/aws-load-balancer-healthcheck-timeout: "5"
        # The amount of time, in seconds, during which no response means a failed
        # health check. This value must be less than the service.beta.kubernetes.io/aws-load-balancer-healthcheck-interval
        # value. Defaults to 5, must be between 2 and 60

        service.beta.kubernetes.io/aws-load-balancer-extra-security-groups: "sg-53fae93f,sg-42efd82e"
        # A list of additional security groups to be added to ELB
```

#### Network Load Balancer support on AWS [alpha]

**Warning:** This is an alpha feature and not recommended for production clusters yet.

Starting in version 1.9.0, Kubernetes supports Network Load Balancer (NLB). To
use a Network Load Balancer on AWS, use the annotation `service.beta.kubernetes.io/aws-load-balancer-type`
with the value set to `nlb`.

```yaml
    metadata:
      name: my-service
      annotations:
        service.beta.kubernetes.io/aws-load-balancer-type: "nlb"
```

Unlike Classic Elastic Load Balancers, Network Load Balancers (NLBs) forward the
client's IP through to the node. If a service's `spec.externalTrafficPolicy` is
set to `Cluster`, the client's IP address will not be propagated to the end
pods.

By setting `spec.externalTrafficPolicy` to `Local`, client IP addresses will be
propagated to the end pods, but this could result in uneven distribution of
traffic. Nodes without any pods for a particular LoadBalancer service will fail
the NLB Target Group's health check on the auto-assigned
`spec.healthCheckNodePort` and not receive any traffic.

In order to achieve even traffic, either use a DaemonSet, or specify a
[pod anti-affinity](/docs/concepts/configuration/assign-pod-node/#inter-pod-affinity-and-anti-affinity-beta-feature)
to not locate pods on the same node.

NLB can also be used with the [internal load balancer](/docs/concepts/services-networking/service/#internal-load-balancer)
annotation.

In order for client traffic to reach instances behind an NLB, the Node security
groups are modified with the following IP rules:

| Rule | Protocol | Port(s) | IpRange(s) | IpRange Description |
|------|----------|---------|------------|---------------------|
| Health Check | TCP | NodePort(s) (`spec.healthCheckNodePort` for `spec.externalTrafficPolicy = Local`) | VPC CIDR | kubernetes.io/rule/nlb/health=\<loadBalancerName\> |
| Client Traffic | TCP | NodePort(s) | `spec.loadBalancerSourceRanges` (defaults to `0.0.0.0/0`) | kubernetes.io/rule/nlb/client=\<loadBalancerName\> |
| MTU Discovery | ICMP | 3,4 | `spec.loadBalancerSourceRanges` (defaults to `0.0.0.0/0`) | kubernetes.io/rule/nlb/mtu=\<loadBalancerName\> |

Be aware that if `spec.loadBalancerSourceRanges` is not set, Kubernetes will
allow traffic from `0.0.0.0/0` to the Node Security Group(s). If nodes have
public IP addresses, be aware that non-NLB traffic can also reach all instances
in those modified security groups.

In order to limit which client IP's can access the Network Load Balancer,
specify `loadBalancerSourceRanges`.

```yaml
spec:
  loadBalancerSourceRanges:
  - "143.231.0.0/16"
```

**Note:** NLB only works with certain instance classes, see the [AWS documentation](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/target-group-register-targets.html#register-deregister-targets)
for supported instance types.


### External IPs

If there are external IPs that route to one or more cluster nodes, Kubernetes services can be exposed on those
`externalIPs`. Traffic that ingresses into the cluster with the external IP (as destination IP), on the service port,
will be routed to one of the service endpoints. `externalIPs` are not managed by Kubernetes and are the responsibility
of the cluster administrator.

In the `ServiceSpec`, `externalIPs` can be specified along with any of the `ServiceTypes`.
In the example below, "`my-service`" can be accessed by clients on "`80.11.12.10:80`"" (`externalIP:port`)

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
  externalIPs:
  - 80.11.12.10
```

## Shortcomings

Using the userspace proxy for VIPs will work at small to medium scale, but will
not scale to very large clusters with thousands of Services.  See [the original
design proposal for portals](http://issue.k8s.io/1107) for more details.

Using the userspace proxy obscures the source-IP of a packet accessing a `Service`.
This makes some kinds of firewalling impossible.  The iptables proxier does not
obscure in-cluster source IPs, but it does still impact clients coming through
a load-balancer or node-port.

The `Type` field is designed as nested functionality - each level adds to the
previous.  This is not strictly required on all cloud providers (e.g. Google Compute Engine does
not need to allocate a `NodePort` to make `LoadBalancer` work, but AWS does)
but the current API requires it.

## API Object

Service is a top-level resource in the Kubernetes REST API. More details about the
API object can be found at: [Service API
object](/docs/api-reference/{{page.version}}/#service-v1-core).

## For More Information

Read [Connecting a Front End to a Back End Using a Service](/docs/tasks/access-application-cluster/connecting-frontend-backend/).
