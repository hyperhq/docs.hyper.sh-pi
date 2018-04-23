# Volume

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


get
------------------------------
List volumes or get a volume

    Usage: pi get volume VOLUME [OPTIONS]

    Get information of volumes

          -e, --access-key                API access key
          -k, --secret-key                API secret key
          -r, --region                    Region name, default: gcp-us-central1
          -u, --user                      Username
          -o, --output                    Output format (json, name)
          --zone                          Availability zone

```sh
//list volumes
$ pi get volumes --zone=gcp-us-central1-b
NAME                 ZONE               SIZE(GB)  CREATEDAT                  POD
vol1                 gcp-us-central1-b  50        2018-03-26T05:31:05+00:00
vol2                 gcp-us-central1-b  2         2018-04-18T07:17:04+00:00  mysql

//get a volume
$ pi get volumes vol1 --output=json
{
    "name": "vol1",
    "size": 50,
    "zone": "gcp-us-central1-b",
    "pod": "mysql",
    "createdAt": "2018-03-26T05:31:05.773Z"
}
```


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
