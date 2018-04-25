delete
------------------------------
Delete one or more floating IP(s)

    Usage: pi delete fips FIP [OPTIONS]

    Release a floating IP

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username

```sh
$ pi delete fip 35.188.x.x 35.192.y.y
fip "35.188.x.x" deleted
fip "35.192.y.y" deleted
```

> If the fip is used by services, then it can not be deleted.
