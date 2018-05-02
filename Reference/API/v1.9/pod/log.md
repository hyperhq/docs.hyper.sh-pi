Get
---------------------------------
get log of the specified Pod

### HTTP Request

`GET /api/v1/namespaces/default/pods/{name}/log`

### Path Parameters
| Parameter | Description |
| --- | --- |
| name | name of the Pod |

### Query Parameter
| Parameter | Description |
| --- | --- |
| container | The container for which to stream logs. Defaults to only container if there is one container in the pod. |
| follow | Follow the log stream of the pod. Defaults to false. |
| limitBytes | If set, the number of bytes to read from the server before terminating the log output. This may not display a complete final line of logging, and may return slightly more or slightly less than the specified limit. |
| previous | Return previous terminated container logs. Defaults to false. |
| sinceSeconds | A relative time in seconds before the current time from which to show logs. If this value precedes the time a pod was started, only logs since the pod start will be returned. If this value is in the future, no logs will be returned. Only one of sinceSeconds or sinceTime may be specified. |
| sinceTime | Only return logs after a specific date (RFC3339). Defaults to all logs. Only one of since-time / since may be used. |
| tailLines | If set, the number of lines from the end of the logs to show. If not specified, logs are shown from the creation of the container or sinceSeconds or sinceTime |
| timestamps | f true, add an RFC3339 or RFC3339Nano timestamp at the beginning of every line of log output. Defaults to false. |


### Response

| Code | Description |
| --- | --- |
| 200 _string_ | OK |
| 400 | Bad Request |
| 404 | Not Found |
| 500 | Server Error |
