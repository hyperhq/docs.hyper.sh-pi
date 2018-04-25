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
