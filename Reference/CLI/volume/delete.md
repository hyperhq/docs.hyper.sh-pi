delete
------------------------------
Delete one or more volume(s)

    Usage: pi delete volumes VOLUME [OPTIONS]

    Delete volume(s)

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username

```sh
$ pi delete volumes vol1 vol2
volume "vol1" deleted
volume "vol2" deleted
```

> If the volume is used by a pod, it can not be deleted.
