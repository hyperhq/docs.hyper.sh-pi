# Floating IP

Floating IP is a public IPv4 address. This resource is allocated by clients and attached to pod.

## Reference

### FipResponse

| Field | Description |
| --- | --- |
| ip: _string_ | The IP address |
| name: _string_ | The name used for remarks |
| createdAt: _string_  | The timestamp when the fip is created |
| services: _string array_  | The name list of the services that the floating IP is related to. Empty if the floating ip is not in use |

### FipNameRequest

| Field | Description |
| --- | --- |
| name | The name used for remarks |


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


List
---------------------------------
list floating IPs

### HTTP Request

`GET /api/v1/hyper/fips/`

### Response

| Code | Description |
| --- | --- |
| 200 _[FipResponse](#fipresponse) array_ | OK |
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
| 200 _[FipResponse](#fipresponse) array_ | OK |
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


Set Name
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
