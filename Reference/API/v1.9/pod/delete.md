Delete
---------------------------------
delete a Pod

### HTTP Request

`DELETE /api/v1/namespaces/default/pods/{name}`

### Path Parameters

| Parameter | Description |
| --- | --- |
| name | name of the Pod |

### Body Parameters

| Parameter | Description |
| --- | --- |
| body _[DeleteOptions](#deleteoptions)_ | support gracePeriodSeconds |

### Response

| Code | Description |
| --- | --- |
| 204 | OK |
| 400 | Bad Request |
| 404 | Not Found |
| 500 | Server Error |
