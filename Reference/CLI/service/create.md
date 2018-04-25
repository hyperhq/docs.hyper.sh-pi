create
------------------------------
Create new service

    Usage: pi create -f FILENAME [OPTIONS]

    Create a new service

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username
              -f, --filename                  Filename, directory, or URL to files to use to create the service, support yaml or json

This command creates a new _service_. If associated with a floating IP, the service will be exposed on the Internet, otherwise the service is internal.

```sh
$ pi create -f service-clusterip-default.yaml
service/test-clusterip-default
```
