Read
---------------------------------
read the specified Event

### HTTP Request

`GET /api/v1/namespaces/default/events`

### Query Parameters

| Parameter | Description |
| --- | --- |
| fieldSelector | A selector to restrict the list of returned objects by their fields. |

Example:
```
1. get events by uid and name
/api/v1/namespaces/default/events?fieldSelector=involvedObject.uid%3D{uid}%2CinvolvedObject.name%3D{name}

2. get events by kine and name
/api/v1/namespaces/default/events?fieldSelector=involvedObject.kind%3D{kind}%2CinvolvedObject.name%3D{name}
```
- {uid} is the uid of resource, {name} is the name of resource, {kind} is the kind of resource
- resource could be pod, service and secret
- {kind} could be Pod, Service, Secret


### Response

| Code | Description |
| --- | --- |
| 200 _[EventList](index.md#eventlist)_ | OK |
| 400 | Bad Request |
| 500 | Server Error |
