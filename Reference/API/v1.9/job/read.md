Read
---------------------------------
read the specified Job

### HTTP Request

`GET /api/v1/namespaces/default/pods/{name}`

### Path Parameters

| Parameter | Description |
| --- | --- |
| name | name of the Pod |

### Response

| Code | Description |
| --- | --- |
| 200 _[Pod](index.md#pod-1)_ | OK |
| 400 | Bad Request |
| 404 | Not Found |
| 500 | Server Error |
