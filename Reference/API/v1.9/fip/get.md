Get
---------------------------------
get a floating IPs

### HTTP Request

`GET /api/v1/hyper/fips/{ip}`

### Path Parameters

| Parameter | Description |
| --- | --- |
| ip | The floating IP to set |

### Response

| Code | Description |
| --- | --- |
| 200 _[FipResponse](index.md#fipresponse) array_ | OK |
| 400 | Bad Request |
| 404 | Not Found |
| 500 | Server Error |


```
{
  "fip": "35.188.x.x",
  "name": "",
  "createdAt": "2018-04-18T08:10:22.9Z",
  "services": [
    "test-loadbalancer-nginx"
  ]
}
```
