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
