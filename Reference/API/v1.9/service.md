# Service v1 core

| Group | Version | Kind |
| --- | --- | --- |
| Core | v1 | Service |

Service is a named abstraction of software service (for example, mysql) consisting of local port (for example 3306) that the proxy listens on, and the selector that determines which pods will answer requests sent through the proxy.

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


Create
------
create a Service

### HTTP Request

`POST /api/v1/namespaces/default/services`

### Body Parameters

| Parameter |
| --- |
| body _[Service](#service)_ |

### Response

| Code | Description |
| --- | --- |
| 201   _[Service](#service)_ | Created |
| 400 | Bad Request |
| 500 | Server Error |

**support type of service**:
- ClusterIP
- LoadBalancer

**not support type of service**:
- ExternalName
- NodePort


List
----
list objects of kind Service

### HTTP Request

`GET /api/v1/namespaces/default/services`


### Response

| Code | Description |
| --- | --- |
| 200 _[ServiceList](#servicelist)_ | OK |
| 400 | Bad Request |
| 500 | Server Error |


Get
----
get the specified Service

### HTTP Request

`GET /api/v1/namespaces/default/services/{name}`

### Path Parameters

| Parameter | Description |
| --- | --- |
| name | name of the Service |

### Response

| Code | Description |
| --- | --- |
| 200   _[Service](#service)_ | OK |
| 400 | Bad Request |
| 404 | Not Found |
| 500 | Server Error |


Delete
------
delete a Service

### HTTP Request

`DELETE /api/v1/namespaces/default/services/{name}`

### Path Parameters

| Parameter | Description |
| --- | --- |
| name | name of the Service |

### Response

| Code | Description |
| --- | --- |
| 204 | OK |
| 400 | Bad Request |
| 404 | Not Found |
| 500 | Server Error |
