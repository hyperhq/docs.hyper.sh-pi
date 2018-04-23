# Floating IP

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



get
------------------------------
List floating IP(s) or get a floating IP


    Usage: pi get fips FIP [OPTIONS]

    get information of floating IPs

          -e, --access-key                API access key
          -k, --secret-key                API secret key
          -r, --region                    Region name, default: gcp-us-central1
          -u, --user                      Username
          -o, --output                    Output format (json, ip)

```sh
//get fip list
$ pi get fip
FIP             NAME  CREATEDAT                  SERVICES
104.197.x.x           2018-04-08T15:27:49+00:00  test-loadbalancer-nginx
104.197.y.y           2018-04-08T15:31:08+00:00
104.197.z.z           2018-04-08T15:31:10+00:00

//get a fip
$ pi get fip 35.188.x.x -o json
{
  "fip": "35.188.x.x",
  "name": "",
  "createdAt": "2018-04-18T08:10:22.9Z",
  "services": [
    "test-loadbalancer-nginx"
  ]
}

```


set name
------------------------------
Set Name of a floating IP

    Usage: pi name fip FIP [OPTIONS]

    Set a name to a floating IP

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username
              -n, --name                      The name of the floating IP

```sh
$ pi name fip 104.197.x.x --name=web
fip "104.197.x.x" named to "web"
```


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
