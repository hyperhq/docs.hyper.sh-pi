delete
------------------------------
Delete one or more pod(s)

    Usage: pi delete pods POD [OPTIONS]

    Delete a pod

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username

```sh
$ pi delete pods busybox
pod "busybox" deleted
```
