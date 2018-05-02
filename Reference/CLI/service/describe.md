describe
------------------------------
Show details of service

    Usage: pi describe services SERVICE [OPTIONS]

    Get a services's detailed description, including related events.

          -e, --access-key                API access key
          -k, --secret-key                API secret key
          -r, --region                    Region name, default: gcp-us-central1
          -u, --user                      Username
          -l, --selector=''               Selector (label query) to filter on, supports '=', '==', and '!='.(e.g. -l key1=value1,key2=value2)
          --include-uninitialized         If true, the pi command applies to uninitialized objects.

```sh
$ pi describe services my-nginx-external
Name:                     my-nginx-external
Namespace:
Labels:                   app=my-nginx-external
Annotations:              id=7cd9de06d9f24464a67222b00182f67bc7c4ac29840e8166e2f20f2d6c3f1819
Selector:                 app=nginx-external
Type:                     LoadBalancer
IP:                       10.96.207.14
IP:                       35.192.x.x
Port:                     8080-80  8080/TCP
TargetPort:               80/TCP
Endpoints:                <none>
Session Affinity:         None
External Traffic Policy:  Cluster
```
