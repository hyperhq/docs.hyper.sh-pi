Read
---------------------------------
read the specified Job

### HTTP Request

`GET /api/v1/namespaces/default/jobs/{name}`

### Path Parameters

| Parameter | Description |
| --- | --- |
| name | name of the Job |

### Response

| Code | Description |
| --- | --- |
| 200 _[Job](index.md#job-1)_ | OK |
| 400 | Bad Request |
| 404 | Not Found |
| 500 | Server Error |
