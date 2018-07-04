Read
----
read the specified Secret

### HTTP Request

`GET /api/v1/namespaces/default/secrets/{name}`

### Path Parameters

| Parameter | Description |
| --- | --- |
| name | name of the Secret |

### Response

| Code | Description |
| --- | --- |
| 200   _[Secret](index.md#secret)_ | OK |
| 400 | Bad Request |
| 404 | Not Found |
| 500 | Server Error |
