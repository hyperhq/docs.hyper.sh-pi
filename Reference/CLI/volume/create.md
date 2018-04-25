create
------------------------------
Create new volume

    Usage: pi create volume VOLUME [OPTIONS]

    Create a new volume

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username
              --size                          Volume size (in GB), 1-1024, default 10
              --zone                          Availability zone

Volume is zone-specific. You may use the `--zone` flag to choose which availability zone the volume will reside in. If is absent, the volume will be created in the default zone of your account. Volume name is region-wise unique, e.g. no two volumes in the same region can have the same volume name.

```sh
$ pi create volume vol1 --size=1 --zone=gcp-us-central1-b
volume/vol1
```
