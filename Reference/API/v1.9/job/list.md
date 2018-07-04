List
---------------------------------
list or watch objects of kind Job

### HTTP Request

`GET /apis/batch/v1/namespaces/default/jobs`

### Query Parameters

| Parameter | Description |
| --- | --- |
| labelSelector | A selector to restrict the list of returned objects by their labels. Defaults to everything. |

### Response

| Code | Description |
| --- | --- |
| 200 _[JobList](index.md#joblist)_ | OK |
| 400 | Bad Request |
| 500 | Server Error |
