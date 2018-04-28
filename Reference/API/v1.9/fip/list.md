List
---------------------------------
list floating IPs

### HTTP Request

`GET /api/v1/hyper/fips/`

### Response

| Code | Description |
| --- | --- |
| 200 _[FipResponse](index.md#fipresponse) array_ | OK |
| 400 | Bad Request |
| 500 | Server Error |

```
[
  {
    "fip": "35.188.x.x",
    "name": "",
    "createdAt": "2018-04-18T08:10:22.9Z",
    "services": [
      "test-loadbalancer-nginx"
    ]
  },
  {
    "fip": "35.192.x.x",
    "name": "",
    "createdAt": "2018-04-18T06:10:41.415Z",
    "services": []
  }
]
```
