create
------------------------------
Create one or more floating IP(s)

    Usage: pi create fip [OPTIONS]

    Allocate new floating IPs

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username
              -c, --count                     Number of floating IPs to be allocated, default 1

```sh
$ pi create fip
fip/104.197.x.x

$ pi create fip --count=2
fip/104.197.x.x
fip/104.197.y.y
```
