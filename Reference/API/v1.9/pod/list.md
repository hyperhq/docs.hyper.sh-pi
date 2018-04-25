List
---------------------------------
list objects of kind Pod

### HTTP Request

`GET /api/v1/namespaces/default/pods`

### Query Parameters

| Parameter | Description |
| --- | --- |
| labelSelector | A selector to restrict the list of returned objects by their labels. Defaults to everything. |

### Response

| Code | Description |
| --- | --- |
| 200 _[PodList](#podlist)_ | OK |
| 400 | Bad Request |
| 500 | Server Error |
