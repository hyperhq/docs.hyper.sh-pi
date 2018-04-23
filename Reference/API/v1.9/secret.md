# Secret

| Group | Version | Kind |
| --- | --- | --- |
| Core | v1 | Secret |

Secret holds secret data of a certain type. The total bytes of the values in the Data field must be less than MaxSecretSize bytes.

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


Create
------
create a Secret

### HTTP Request

`POST /api/v1/namespaces/default/secrets`

### Body Parameters

| Parameter |
| --- |
| body _[Secret](#secret)_ |

### Response

| Code | Description |
| --- | --- |
| 201   _[Secret](#secret)_ | Created |
| 400 | Bad Request |
| 500 | Server Error |


List
----
list objects of kind Secret

### HTTP Request

`GET /api/v1/namespaces/default/secrets`


### Response

| Code | Description |
| --- | --- |
| 200 _[SecretList](#secretlist)_ | OK |
| 400 | Bad Request |
| 500 | Server Error |


Get
----
get the specified Secret

### HTTP Request

`GET /api/v1/namespaces/default/secrets/{name}`

### Path Parameters

| Parameter | Description |
| --- | --- |
| name | name of the Secret |

### Response

| Code | Description |
| --- | --- |
| 200   _[Secret](#secret)_ | OK |
| 400 | Bad Request |
| 404 | Not Found |
| 500 | Server Error |


Delete
------
delete a Secret

### HTTP Request

`DELETE /api/v1/namespaces/default/secrets/{name}`

### Path Parameters

| Parameter | Description |
| --- | --- |
| name | name of the Secret |

### Response

| Code | Description |
| --- | --- |
| 204  | OK |
| 400 | Bad Request |
| 404 | Not Found |
| 500 | Server Error |
