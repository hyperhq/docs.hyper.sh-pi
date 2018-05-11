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
//create service from file
$ pi create -f service-clusterip-default.yaml
service/test-clusterip-default

//create via subcommand (clusterip service)
$ pi create service clusterip nginx-internal --tcp=8080:80 --selector=app=nginx-internal
service/nginx-internal

//create via subcommand (loadbalancer service, '--loadbalancerip' is floating IP)
$ pi create service loadbalancer my-nginx-external --tcp=8080:80 --loadbalancerip=35.193.x.x --selector=app=nginx-external
service/my-nginx-external
```
