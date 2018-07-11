# Floating IP

Floating IP is a public IPv4 address. This resource is allocated by clients and attached to pod.

## METHOD
- [Create](create.md)
- [List](list.md)
- [Read](read.md)
- [Name](name.md)
- [Delete](delete.md)

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
