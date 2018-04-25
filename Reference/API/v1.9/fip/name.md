Name
---------------------------------
set a name for floating IP

### HTTP Request

`POST /api/v1/hyper/fips/{ip}`

### Path Parameters

| Parameter | Description |
| --- | --- |
| ip | The floating IP to set |

### Body Parameters

| Parameter | Description |
| --- | --- |
| [FipNameRequest](#fipnamerequest) | |

### Response

| Code | Description |
| --- | --- |
| 204 | OK |
| 404 | Not Found |
| 500 | Server Error |
