# Secret

In _Pi_, objects of type `secret` are intended to hold sensitive information. Putting this information in a `secret` is safer and more flexible than putting it verbatim in a `pod` definition or in a docker image. See [Secrets design document](https://git.k8s.io/community/contributors/design-proposals/auth/secrets.md) for more information.

> Currently, we only support Docker registry secret, which is equivalent to [_kubectl create secret docker-registry_](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-secret-docker-registry-em-).

_Secret_ is regional, e.g. you need to create secrets in different regions separately.

### Creating a Docker registry secret

To create a new secret, use:
```
$ pi secret create my-secret --docker-server=DOCKER_REGISTRY_SERVER --docker-username=DOCKER_USER --docker-password=DOCKER_PASSWORD --docker-email=DOCKER_EMAIL
```
- `docker-email`:		  Email for Docker registry
- `docker-password`:	Password for Docker registry authentication
- `docker-server`:		Server location for Docker registry (Default: https://index.docker.io/v1/)
- `docker-username`: 	Username for Docker registry authentication

### Referring to an imagePullSecrets on a Pod

Now, you can create pods which reference that secret by adding an `imagePullSecrets` section to a pod definition.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: foo
  namespace: awesomeapps
spec:
  containers:
    - name: foo
      image: janedoe/awesomeapp:v1
  imagePullSecrets:
    - name: myregistrykey
```

### Limits

- Max secret size: 4kb
- Max secrets per region: 8

### Secret and Pod Lifetime interaction

When a pod is created via the API, there is no check whether a referenced secret exists.  Once a pod is scheduled, the kubelet will try to fetch the secret value.  If the secret cannot be fetched because it does not exist or because of a temporary lack of connection to the API server, the system will periodically retry until the secret is successfully fetched.
