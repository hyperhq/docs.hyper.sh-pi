name
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
