List
---------------------------------
List volumes

### HTTP Request

`GET /api/v1/hyper/volumes/`

### Response

| Code | Description |
| --- | --- |
| 200 _[VolumeResponse](index.md#volumeresponse) array_ | OK |
| 400 | Bad Request |
| 500 | Server Error |

```
[
  {
    "name": "pt-test-performance",
    "size": 50,
    "zone": "gcp-us-central1-a",
    "pod": "",
    "createdAt": "2018-03-26T05:31:05.773Z"
  },
  {
    "name": "vol1",
    "size": 2,
    "zone": "gcp-us-central1-a",
    "pod": "mysql",
    "createdAt": "2018-04-18T07:17:04.242Z"
  }
]
```
