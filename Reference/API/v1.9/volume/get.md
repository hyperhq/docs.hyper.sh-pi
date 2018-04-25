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
