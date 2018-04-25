create
------------------------------
Create new pod

    Usage: pi [OPTIONS] create -f FILENAME

    Create a new pod

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username
              -f, --filename                  Filename, directory, or URL to files to use to create the pod, support yaml or json

The `create` command submits the pod request specified in the pod spec file to _Pi_.

```sh
$ pi create -f pod-nginx.yaml
pod/nginx
```
