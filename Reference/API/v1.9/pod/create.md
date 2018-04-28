Create
---------------------------------
create a Pod

### HTTP Request

`POST /api/v1/namespaces/default/pods`

### Body Parameters

| Parameter |
| --- |
| body _[Pod](index.md#pod-1)_ |

### Response

| Code | Description |
| --- | --- |
| 201 _[Pod](index.md#pod-1)_ | Created |
| 400 | Bad Request |
| 500 | Server Error |
