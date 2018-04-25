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
