# Secret

| Group | Version | Kind |
| --- | --- | --- |
| Core | v1 | Secret |

Secret holds secret data of a certain type. The total bytes of the values in the Data field must be less than MaxSecretSize bytes.

## METHOD
- [create](create.md)
- [list](list.md)
- [get](get.md)
- [delete](delete.md)

## Reference:

### Secret

- [Secret/v1 core](https://v1-9.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.9/#secret-v1-core)

**Secret Types**
- **Supported**
  - **"Opaque"**
  - **"kubernetes.io/dockercfg"**,**"kubernetes.io/dockerconfigjson"**
- **Not Support**
  - ~~"kubernetes.io/service-account-token"~~

### SecretList

- [SecretList/v1 core](https://v1-9.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.9/#secretlist-v1-core)
