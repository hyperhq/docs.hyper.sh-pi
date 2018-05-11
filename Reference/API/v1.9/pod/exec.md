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
| [PodExecConfig](index.md#podexecconfig) |

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
| 201 _[PodExecCreateResponse](index.md#podexeccreateresponse)_ | OK |
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
| [PodExecConfig](index.md#podexecconfig) |

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

`POST /api/v1/exec/{execID}/resize?h={height}&w={width}`

#### Path Parameters

| Parameter | Description |
| --- | --- |
| execID | id of exec process |

#### Query Parameters

| Parameter | Description |
| --- | --- |
| height | tty height |
| width | tty width |
