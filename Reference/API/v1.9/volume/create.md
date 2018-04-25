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
