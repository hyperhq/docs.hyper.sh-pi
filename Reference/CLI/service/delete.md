delete
------------------------------
Delete one or more service(s)

    Usage: pi delete services SERVICE [OPTIONS]

    Delete a service

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username
              --all                           Delete all resources, including uninitialized ones, in the namespace of the specified resource types.

```sh
$ pi delete services http
secret "http" deleted
```
