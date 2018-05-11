# Service v1 core

| Group | Version | Kind |
| --- | --- | --- |
| Core | v1 | Service |

Service is a named abstraction of software service (for example, mysql) consisting of local port (for example 3306) that the proxy listens on, and the selector that determines which pods will answer requests sent through the proxy.

## METHOD
- [Create](create.md)
- [List](list.md)
- [Get](get.md)
- [Delete](delete.md)

## Reference:

### Service

- [Service/v1 core](https://v1-9.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.9/#service-v1-core)

**Service type**:
- **Supported**
  - ClusterIP
  - LoadBalalancer
- **Not Support**
  - ~~NodePort~~ (not support)
  - ~~ExternalName~~ (not support)

### ServiceSpec

- [ServiceSpec/v1 core](https://v1-9.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.9/#servicespec-v1-core)

**ServiceSpec Properties**:
- **Supported**
  - **clusterIP** string
  - **loadBalancerIP** string
  - **ports** ServicePort array
    - **name** string
    - **protocol** string
    - **port** integer
    - **targetPort** integer
    - ~~nodePort~~ integer
  - **publishNotReadyAddresses** boolean
  - **selector** object
  - **sessionAffinity** string
  - **sessionAffinityConfig** SessionAffinityConfig
  - **type** string
    - **"ClusterIP"**
    - **"LoadBalalancer"**
    - ~~"NodePort"~~
    - ~~"ExternalName"~~
- **Not Support**
  - ~~externalIPs~~ string array (`reserved, should be empty`)
  - ~~externalName~~ string
  - ~~externalTrafficPolicy~~ string
  - ~~healthCheckNodePort~~ integer
  - ~~loadBalancerSourceRanges~~ string array


### ServiceList

- [ServiceList/v1 core](https://v1-9.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.9/#servicelist-v1-core)
