# Pod

| Group | Version | Kind |
| --- | --- | --- |
| Core | v1 | Pod |

Pod is a collection of containers that can run on a host. This resource is created by clients and scheduled onto hosts.


## Reference:

### Pod

- [Pod/v1 core](https://v1-9.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.9/#pod-v1-core)

### PodSpec

- [PodSpec/v1 core](https://v1-9.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.9/#podspec-v1-core)

**PodSpec Properties**:

- **Supported**
  - **activeDeadlineSeconds** integer
  - **affinity** Affinity
  - **containers** Container array
  - **hostAliases** HostAlias array
  - **hostname** string
  - **imagePullSecrets** LocalObjectReference array
  - **initContainers** Container array
  - **nodeSelector** object
  - **restartPolicy** string
  - **securityContext** PodSecurityContext
    - **runAsUser** integer
    - **supplementalGroups** integer array
    - ~~seLinuxOptions~~ SELinuxOptions
    - ~~fsGroup~~ integer
    - ~~runAsNonRoot~~ boolean
  - **subdomain** string
  - **terminationGracePeriodSeconds** integer
  - **volumes** Volume array

- **Not Support**
  - ~~automountServiceAccountToken~~ boolean
  - ~~deprecatedServiceAccount~~ string
  - ~~dnsPolicy~~ string
  - ~~dnsConfig~~ PodDNSConfig
  - ~~hostNetwork~~ boolean
  - ~~hostPID~~ boolean
  - ~~hostIPC~~ boolean
  - ~~nodeName~~ string
  - ~~priorityClassName~~ string
  - ~~priority~~ integer
  - ~~schedulerName~~ string
  - ~~serviceAccountName~~ string
  - ~~tolerations~~ Toleration array

### Container

- [Container/v1 core](https://v1-9.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.9/#container-v1-core)

**Container Properties**:
- **Supported**
  - **args** string array
  - **command** string array
  - **env** EnvVar array
  - **envFrom** EnvFromSource array
  - **image** string
  - **imagePullPolicy** string
  - **lifecycle** Lifecycle
  - **livenessProbe** Probe
  - **name** string
  - **ports** ContainerPort array
  - **readinessProbe** Probe
  - **resources** ResourceRequirements
  - **securityContext** SecurityContext
    - **allowPrivilegeEscalation** boolean
    - **readOnlyRootFilesystem** boolean
    - **runAsNonRoot** boolean
    - ~~capabilities~~ Capabilities
    - ~~privileged~~ boolean
    - ~~runAsUser~~ integer
    - ~~seLinuxOptions~~ SELinuxOptions
  - **stdin** boolean
  - **stdinOnce** boolean
  - **terminationMessagePath** string
  - **terminationMessagePolicy** string
  - **tty** boolean
  - **volumeMounts** VolumeMount array
    - **name** string
    - **readOnly** boolean
    - **mountPath** string
    - **subPath** string
    - ~~mountPropagation~~ MountPropagationMode
  - **workingDir** string


### FlexVolumeSource

- [FlexVolumeSource/v1 core](https://v1-9.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.9/#flexvolumesource-v1-core)

**FlexVolumeSource Properties**:
- **Support**
  - options object
    - **volumeID** string (`volume name`)
  - readOnly boolean
  - secretRef LocalObjectReference
- **Not Support**
  - ~~driver~~ string
  - ~~fsType~~ string


### PodList

- [PodList/v1 core](https://v1-9.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.9/#podlist-v1-core)

### DeleteOptions

- [DeleteOptions/v1 meta](https://v1-9.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.9/#deleteoptions-v1-meta)

### PodExecConfig

| Field | Type | Description |
| --- | --- | --- |
| tty | _bool_ | allocate a pseudo-TTY |
| attachStdin | _bool_ | Attach the standard input, makes possible user interaction |
| attachStderr | _bool_ | Attach the standard output |
| attachStdout | _bool_ | Attach the standard error |
| cmd | _string array_ | command to run |

### PodExecCreateResponse

| Field | Type | Description |
| --- | --- | --- |
| Id | _bool_ | id of exec process |


Create
---------------------------------
create a Pod

### HTTP Request

`POST /api/v1/namespaces/default/pods`

### Body Parameters

| Parameter |
| --- |
| body _[Pod](#pod-1)_ |

### Response

| Code | Description |
| --- | --- |
| 201 _[Pod](#pod-1)_ | Created |
| 400 | Bad Request |
| 500 | Server Error |


List
---------------------------------
list objects of kind Pod

### HTTP Request

`GET /api/v1/namespaces/default/pods`

### Query Parameters

| Parameter | Description |
| --- | --- |
| labelSelector | A selector to restrict the list of returned objects by their labels. Defaults to everything. |

### Response

| Code | Description |
| --- | --- |
| 200 _[PodList](#podlist)_ | OK |
| 400 | Bad Request |
| 500 | Server Error |


Get
---------------------------------
get the specified Pod

### HTTP Request

`GET /api/v1/namespaces/default/pods/{name}`

### Path Parameters

| Parameter | Description |
| --- | --- |
| name | name of the Pod |

### Response

| Code | Description |
| --- | --- |
| 200 _[Pod](#pod-1)_ | OK |
| 400 | Bad Request |
| 404 | Not Found |
| 500 | Server Error |


Exec
---------------------------------
exec a command in container

### ExecCreate
creates a new exec configuration to run an exec process

#### HTTP Request

`POST /api/v1/namespaces/default/pods/{podName}/exec?container={containerName}`

#### Path Parameters

| Parameter | Description |
| --- | --- |
| podName | name of pod |

#### Query Parameters

| Parameter | Description |
| --- | --- |
| containerName | name of container |

#### Body Parameters

| Parameter |
| --- |
| [PodExecConfig](#podexecconfig) |

```
$ pi exec nginx -c nginx -- echo hello
//Post Data
{"tty":false,"attachStdin":false,"attachStderr":true,"attachStdout":true, "cmd":["echo","hello"]}

$ pi exec nginx -it -c nginx -- sh
//Post Data
{"tty":true,"attachStdin":true,"attachStderr":true,"attachStdout":true,"cmd":["sh"]}
```

#### Response

| Code | Description |
| --- | --- |
| 201 _[PodExecCreateResponse](#podexeccreateresponse)_ | OK |
| 400 | Bad Request |
| 404 | Not Found |
| 500 | Server Error |

```
{
  "Id": "exec-mtaUcLDzRk"
}
```

### ExecStart
starts an exec process already created

#### HTTP Request

`POST /api/v1/exec/{execID}/start`

#### Path Parameters

| Parameter | Description |
| --- | --- |
| execID | id of exec process |

#### Body Parameters

| Parameter |
| --- |
| [PodExecConfig](#podexecconfig) |

#### Response

| Code | Description |
| --- | --- |
| 101 | No error, hints proxy about hijacking |
| 200 | No error, no upgrade header found |
| 400 | Bad Request |
| 404 | Not Found |
| 500 | Server Error |

{{ STREAM }}


**Stream details**:

When using the TTY setting is enabled in POST /api/v1/exec/{execID}/start , the stream is the raw data from the process PTY and client's stdin. When the TTY is disabled, then the stream is multiplexed to separate stdout and stderr.

The format is a Header and a Payload (frame).

**HEADER**

The header contains the information which the stream writes (stdout or stderr). It also contains the size of the associated frame encoded in the last four bytes (uint32).

It is encoded on the first eight bytes like this:

    header := [8]byte{STREAM_TYPE, 0, 0, 0, SIZE1, SIZE2, SIZE3, SIZE4}

STREAM_TYPE can be:

    0: stdin (is written on stdout)
    1: stdout
    2: stderr

SIZE1, SIZE2, SIZE3, SIZE4 are the four bytes of the uint32 size encoded as big endian.

**PAYLOAD**

The payload is the raw stream.

**IMPLEMENTATION**

The simplest way to implement the Attach protocol is the following:

1. Read eight bytes.
2. Choose stdout or stderr depending on the first byte.
3. Extract the frame size from the last four bytes.
4. Read the extracted size and output it on the correct output.
5. Goto 1.


### ExecResize
changes the size of the tty

#### HTTP Request

`/api/v1/exec/{execID}/resize?h={height}&w={width}`

#### Path Parameters

| Parameter | Description |
| --- | --- |
| execID | id of exec process |

#### Query Parameters

| Parameter | Description |
| --- | --- |
| height | tty height |
| width | tty width |


Delete
---------------------------------
delete a Pod

### HTTP Request

`DELETE /api/v1/namespaces/default/pods/{name}`

### Path Parameters

| Parameter | Description |
| --- | --- |
| name | name of the Pod |

### Body Parameters

| Parameter | Description |
| --- | --- |
| body _[DeleteOptions](#deleteoptions)_ | support gracePeriodSeconds |

### Response

| Code | Description |
| --- | --- |
| 204 | OK |
| 400 | Bad Request |
| 404 | Not Found |
| 500 | Server Error |
