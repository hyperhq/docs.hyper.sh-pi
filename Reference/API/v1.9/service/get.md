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
