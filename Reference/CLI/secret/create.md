create
------------------------------
Create new secret

### create secret from file

    Usage: pi create -f FILENAME [OPTIONS]

    Create a new secret

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username
              -f, --filename                  Filename, directory, or URL to files to use to create the secret, support yaml or json

This command creates a new __Docker registry__ secret.

```sh
//create file yaml file
$ pi create -f examples/secret/secret-dockerconfigjson.yaml
secret/test-secret-dockerconfigjson

//create via subcommand
$ pi create secret docker-registry my-secret \
 --docker-server=DOCKER_REGISTRY_SERVER\
 --docker-username=DOCKER_USER\
 --docker-password=DOCKER_PASSWORD\
 --docker-email=DOCKER_EMAIL
```

### create docker-registry

    Usage: pi create secret docker-registry NAME --docker-username=user --docker-password=password --docker-email=email [--docker-server=string] [OPTIONS]

    create a docker registry secret

              --append-hash                   Append a hash of the secret to its name.
              --docker-email                  Email for Docker registry
              --docker-password               Password for Docker registry authentication
              --docker-server                 Server location for Docker registry
              --docker-username               Username for Docker registry authentication

```sh
$ pi create secret docker-registry my-secret \
 --docker-server=DOCKER_REGISTRY_SERVER\
 --docker-username=DOCKER_USER\
 --docker-password=DOCKER_PASSWORD\
 --docker-email=DOCKER_EMAIL
```
