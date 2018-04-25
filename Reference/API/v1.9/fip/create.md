Create
---------------------------------
allocate floating IP

### HTTP Request

`POST /api/v1/hyper/fips/`

### Query Parameters

| Parameter | Description |
| --- | --- |
| count | The number of new floating IPs to allocate |

### Response

| Code | Description |
| --- | --- |
| 201 _[FipResponse](#fipresponse) array_ | OK |
| 400 | Bad Request |
| 500 | Server Error |

```
[
  {
    "fip": "35.192.x.x",
    "name": "",
    "createdAt": "2018-04-18T02:44:49.019-07:00",
    "services": []
  }
]
```
