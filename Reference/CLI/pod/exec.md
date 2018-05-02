exec
------------------------------
Execute command in container

    Usage: pi exec POD [OPTIONS] [-c CONTAINER] -- COMMAND [args...]

    Execute a command in a container

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username
              -c, --container                 Container name. If omitted, the first container in the pod will be chosen
              -i, --stdin                     Pass stdin to the container
              -t, --tty                       Stdin is a TTY

If no container specified, the command will be executed in the first container defined in the pod spec.

```sh
$ pi exec multiple-busybox-alpine -c alpine -- ping -c3 35.226.x.x
PING 35.226.x.x (35.226.x.x): 56 data bytes
64 bytes from 35.226.x.x: seq=0 ttl=64 time=2.075 ms
64 bytes from 35.226.x.x: seq=1 ttl=64 time=1.600 ms
64 bytes from 35.226.x.x: seq=2 ttl=64 time=2.009 ms
...

$ pi exec -it mysql -c mysql -- bash
root@mysql:/# uname -r
4.12.4-hyper
root@mysql:/# exit
```
