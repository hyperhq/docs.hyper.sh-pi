describe
------------------------------
Show details of secret

    Usage: pi describe secrets SECRET [OPTIONS]

    Get a secret's detailed description, including related events.

          -e, --access-key                API access key
          -k, --secret-key                API secret key
          -r, --region                    Region name, default: gcp-us-central1
          -u, --user                      Username
          -l, --selector=''               Selector (label query) to filter on, supports '=', '==', and '!='.(e.g. -l key1=value1,key2=value2)
          --include-uninitialized         If true, the pi command applies to uninitialized objects.

```sh
$ pi describe secrets my-generic-secret
Name:         my-generic-secret
Namespace:
Labels:       <none>
Annotations:  id=c495c6bddf206723968bb1301a7bf369bfde132df516b1756384eb5f7a064f03

Type:  Opaque

Data
====
key1:  11 bytes
key2:  9 bytes
```
