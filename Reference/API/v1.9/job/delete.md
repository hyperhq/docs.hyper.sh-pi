Delete
---------------------------------
delete a Job

### HTTP Request

`DELETE /apis/batch/v1/namespaces/default/jobs/{name}`

### Path Parameters

| Parameter | Description |
| --- | --- |
| name | name of the Job |

### Body Parameters

| Parameter | Description |
| --- | --- |
| body _[DeleteOptions](https://v1-9.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.9/#deleteoptions-v1-meta)_ | support gracePeriodSeconds |

### Response

| Code | Description |
| --- | --- |
| 200 | OK |
| 400 | Bad Request |
| 404 | Not Found |
| 500 | Server Error |
