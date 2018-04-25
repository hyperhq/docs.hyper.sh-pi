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
