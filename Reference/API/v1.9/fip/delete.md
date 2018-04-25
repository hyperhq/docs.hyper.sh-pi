Delete
---------------------------------
release floating IP

### HTTP Request

`DELETE /api/v1/hyper/fips/{ip}`

### Path Parameters

| Parameter | Description |
| --- | --- |
| ip | The floating IP to release |

### Response

| Code | Description |
| --- | --- |
| 204 | OK |
| 404 | Not Found |
| 409 | In Use |
| 500 | Server Error |

> If the fip is used by services, then it can not be deleted.
