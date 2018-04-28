Create
------
create a Service

### HTTP Request

`POST /api/v1/namespaces/default/services`

### Body Parameters

| Parameter |
| --- |
| body _[Service](index.md#service)_ |

### Response

| Code | Description |
| --- | --- |
| 201   _[Service](index.md#service)_ | Created |
| 400 | Bad Request |
| 500 | Server Error |

**support type of service**:
- ClusterIP
- LoadBalancer

**not support type of service**:
- ExternalName
- NodePort
