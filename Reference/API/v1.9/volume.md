# Volume

Volume is a persistent storage to store data. This resource is created by clients and mounted to pod.

## Reference

### VolumeCreateRequest

| Field | Description |
| --- | --- |
| name: _string_ | The volume name, unique in the region where the volume resides |
| size: _integer_ | The volume size in GB, range from 1 to 1024 (GB), default is 10 |
| zone: _string_  | The name of the availability zone where the volume is created. Optional for creating volumes, if empty,

### VolumeResponse

| Field | Description |
| --- | --- |
| name: _string_ | The volume name |
| size: _integer_ | The volume size in GB|
| zone: _string_  | The name of the availability zone where the volume is created. |
| createdAt: _string_  | The timestamp when the volume is created |
| pod: _string_  | The name of the pod that the volume is mounted at. Empty if the volume is not in use |


Create
------------------------------
create a volume

### HTTP Request

`POST /api/v1/hyper/volumes/`

### Body Parameters

| Parameter | Description |
| --- | --- |
| [VolumeCreateRequest](#volumecreaterequest) | |

### Response

| Code | Description |
| --- | --- |
| 201 _[FIP](#VolumeResponse)_ | OK |
| 400 | Bad Request |
| 500 | Server Error |


List
---------------------------------
List volumes

### HTTP Request

`GET /api/v1/hyper/volumes/`

### Response

| Code | Description |
| --- | --- |
| 200 _[VolumeResponse](#volumeresponse) array_ | OK |
| 400 | Bad Request |
| 500 | Server Error |

```
[
  {
    "name": "pt-test-performance",
    "size": 50,
    "zone": "gcp-us-central1-b",
    "pod": "",
    "createdAt": "2018-03-26T05:31:05.773Z"
  },
  {
    "name": "vol1",
    "size": 2,
    "zone": "gcp-us-central1-b",
    "pod": "mysql",
    "createdAt": "2018-04-18T07:17:04.242Z"
  }
]
```

Get
---------------------------------
Get the specified volume

### HTTP Request

`GET /api/v1/hyper/volumes/{name}`

### Query Parameters

| Parameter | Description |
| --- | --- |
| name | The volume name |

### Response

| Code | Description |
| --- | --- |
| 200 _[VolumeResponse](#volumeresponse)_ | OK |
| 400 | Bad Request |
| 404 | Not Found |
| 500 | Server Error |

```
{
  "name": "vol1",
  "size": 2,
  "zone": "gcp-us-central1-b",
  "pod": "mysql",
  "createdAt": "2018-04-18T07:17:04.242Z"
}
```


Delete
---------------------------------
delete a volume

### HTTP Request

`DELETE /api/v1/hyper/volumes/{name}`

### Path Parameters

| Parameter | Description |
| --- | --- |
| name | The volume name |

### Response

| Code | Description |
| --- | --- |
| 204 | Deleted |
| 404 | Not Found |
| 409 | In Use |
| 500 | Server Error |

> If the volume is used by a pod, it can not be deleted.
