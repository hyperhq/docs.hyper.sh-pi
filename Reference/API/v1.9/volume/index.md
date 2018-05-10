# Volume

Volume is a persistent storage to store data. This resource is created by clients and mounted to pod.

## METHOD
- [Create](create.md)
- [List](list.md)
- [Get](get.md)
- [Delete](delete.md)

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
