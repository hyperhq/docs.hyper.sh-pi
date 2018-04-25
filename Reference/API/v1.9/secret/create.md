Create
------
create a Secret

### HTTP Request

`POST /api/v1/namespaces/default/secrets`

### Body Parameters

| Parameter |
| --- |
| body _[Secret](#secret)_ |

### Response

| Code | Description |
| --- | --- |
| 201   _[Secret](#secret)_ | Created |
| 400 | Bad Request |
| 500 | Server Error |
