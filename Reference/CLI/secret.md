# Secret

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



get
------------------------------
list secrets or get a secret

    Usage: pi get secrets SECRET [OPTIONS]

    Get information of secrets

          -e, --access-key                API access key
          -k, --secret-key                API secret key
          -r, --region                    Region name, default: gcp-us-central1
          -u, --user                      Username
          -o, --output                    Output format (json, yaml, name, wide)

```sh
//list secrets
$ pi get secrets
NAME                           TYPE                             DATA      AGE
test-secret-dockerconfigjson   kubernetes.io/dockerconfigjson   1         2m
test-secret-gitlab             kubernetes.io/dockerconfigjson   1         8s

//get a secret
$ pi get secrets test-secret-dockerconfigjson -o yaml
apiVersion: v1
data:
  .dockerconfigjson: eyJhdXRocyI6eyJyZWdpc3RyeS5jbi1oYW5nemhvdS5hbGl5dW5jcy5jb20veHh4eC9yYmQtcmVzdC1hcGkiOnsidXNlcm5hbWUiOiJ0ZXN0IiwicGFzc3dvcmQiOiJ0ZXN0IiwiZW1haWwiOiJ0ZXN0QHRlc3QuY29tIiwiYXV0aCI6ImRHVnpkRHAwWlhOMCJ9fX0=
kind: Secret
metadata:
  annotations:
    id: 5e487f6997947b1a4709cda05f6f9568a7a0873fe1050c6d1d2330bcdd159969
  creationTimestamp: 2018-04-18T10:26:54Z
  name: test-secret-dockerconfigjson
  namespace: default
  resourceVersion: "11176421"
  selfLink: /v1/namespaces/default/secrets/test-secret-dockerconfigjson
  uid: 03c06c18-42f3-11e8-b8a4-42010a000032
type: kubernetes.io/dockerconfigjson
```



delete
------------------------------
Delete one or more secret(s)

    Usage: pi delete secrets SECRET [OPTIONS]

    Delete a secret

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username

```sh
$ pi delete secrets test-secret-gitlab
secret "test-secret-gitlab" deleted
```
