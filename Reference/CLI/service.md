# Service

create
------------------------------
Create new service

    Usage: pi create -f FILENAME [OPTIONS]

    Create a new service

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username
              -f, --filename                  Filename, directory, or URL to files to use to create the service, support yaml or json

This command creates a new _service_. If associated with a floating IP, the service will be exposed on the Internet, otherwise the service is internal.

```sh
$ pi create -f service-clusterip-default.yaml
service/test-clusterip-default
```



get
------------------------------
List services or get a service


    Usage: pi get services SERVICE [OPTIONS]

    Get information of services

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username
              -o, --output                    Output format (json, yaml, name, wide)

```sh
//list services
$ pi get services
NAME                          TYPE           CLUSTER-IP      LOADBALANCER-IP   PORT(S)             AGE
test-clusterip-nginx          ClusterIP      10.109.216.15   <none>            8080/TCP,8080/UDP   20m
test-loadbalancer-mysql       LoadBalancer   10.102.5.104    35.188.x.x        3306/TCP            26m

//get a service
$ pi get services test-loadbalancer-nginx -o yaml
apiVersion: v1
kind: Service
metadata:
  annotations:
    id: 25b8b77a66e73753dff223c70c8308648f65a4b76f115d14fb2fd08423b7b621
  creationTimestamp: 2018-04-18T09:42:32Z
  name: test-loadbalancer-nginx
  namespace: default
  resourceVersion: "11173165"
  selfLink: /v1/namespaces/default/services/test-loadbalancer-nginx
  uid: d13e2052-42ec-11e8-b8a4-42010a000032
spec:
  clusterIP: 10.108.203.249
  loadBalancerIP: 35.188.x.x
  ports:
  - name: tcp-80
    port: 8080
    protocol: TCP
    targetPort: 80
  - name: tcp-443
    port: 6443
    protocol: TCP
    targetPort: 443
  selector:
    app: nginx
  type: LoadBalancer
status:
  loadBalancer: {}
```

> `spec.loadBalancerIP` is `fip`



delete
------------------------------
Delete one or more service(s)

    Usage: pi delete services SERVICE [OPTIONS]

    Delete a service

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username

```sh
$ pi delete services http
secret "http" deleted
```
