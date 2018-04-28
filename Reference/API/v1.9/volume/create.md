Create
------------------------------
create a volume

### HTTP Request

`POST /api/v1/hyper/volumes/`

### Body Parameters

| Parameter | Description |
| --- | --- |
| [VolumeCreateRequest](index.md#volumecreaterequest) | |

### Response

| Code | Description |
| --- | --- |
| 201 _[FIP](index.md#VolumeResponse)_ | OK |
| 400 | Bad Request |
| 500 | Server Error |
