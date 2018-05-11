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
$ pi get volumes --zone=gcp-us-central1-a
NAME                 ZONE               SIZE(GB)  CREATEDAT                  POD
vol1                 gcp-us-central1-a  50        2018-03-26T05:31:05+00:00
vol2                 gcp-us-central1-a  2         2018-04-18T07:17:04+00:00  mysql

//get a volume
$ pi get volumes vol1 --output=json
{
    "name": "vol1",
    "size": 50,
    "zone": "gcp-us-central1-a",
    "pod": "mysql",
    "createdAt": "2018-03-26T05:31:05.773Z"
}
```
