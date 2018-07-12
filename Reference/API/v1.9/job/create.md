Create
---------------------------------
create a Job

### HTTP Request

`POST /apis/batch/v1/namespaces/default/jobs`

### Body Parameters

| Parameter |
| --- |
| body _[Job](index.md#job-1)_ |

### Response

| Code | Description |
| --- | --- |
| 201 _[Job](index.md#job-1)_ | Created |
| 202 _[Job](index.md#job-1)_ | Accepted |
| 200 _[Job](index.md#job-1)_ | OK |
| 400 | Bad Request |
| 500 | Server Error |
