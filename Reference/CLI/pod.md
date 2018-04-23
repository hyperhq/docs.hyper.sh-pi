# Pod

create
------------------------------
Create new pod

    Usage: pi [OPTIONS] create -f FILENAME

    Create a new pod

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username
              -f, --filename                  Filename, directory, or URL to files to use to create the pod, support yaml or json

The `create` command submits the pod request specified in the pod spec file to _Pi_.

```sh
$ pi create -f pod-nginx.yaml
pod/nginx
```


get
------------------------------
List pods or get a pod

    Usage: pi get pods POD [OPTIONS]

    Get a pod's detailed information

          -e, --access-key                API access key
          -k, --secret-key                API secret key
          -r, --region                    Region name, default: gcp-us-central1
          -u, --user                      Username
          -o, --output                    Output format (json, yaml, name, wide)
          -l, --selector=''               Selector (label query) to filter on, supports '=', '==', and '!='.(e.g. -l key1=value1,key2=value2)
          -a, --show-all=false            When printing, show all resources (default hide terminated pods.)


```sh
//list pods
$ pi get pods -l app=nginx
NAME      READY     STATUS    RESTARTS   AGE
nginx     1/1       Running   0          2h

//get a pod
$ pi get pods mysql -o yaml
apiVersion: v1
kind: Pod
metadata:
  annotations:
    id: 0ab3e519e570e25635b3835f2584ab61ff8f82f6c6fb6e656c41b40390ab6979
    sh_hyper_instancetype: s4
    zone: gcp-us-central1-b
  creationTimestamp: 2018-04-19T03:25:57Z
  labels:
    name: mysql
  name: mysql
  namespace: default
  resourceVersion: "11251177"
  selfLink: /v1/namespaces/default/pods/mysql
  uid: 5fee417a-4381-11e8-b8a4-42010a000032
spec:
  containers:
  - args:
    - --ignore-db-dir
    - lost+found
    env:
    - name: MYSQL_ROOT_PASSWORD
      value: make-vm-great-again
    image: mysql
    name: mysql
    ports:
    - containerPort: 3306
      name: mysql
    resources: {}
    volumeMounts:
    - mountPath: /var/lib/mysql
      name: mysql-persistent-storage
  nodeName: gcp-us-central1
  volumes:
  - flexVolume:
      driver: ""
      options:
        volumeID: vol1
    name: mysql-persistent-storage
status:
  conditions:
  - lastProbeTime: null
    lastTransitionTime: 2018-04-19T03:25:57Z
    status: "True"
    type: Initialized
  - lastProbeTime: null
    lastTransitionTime: 2018-04-19T03:26:03Z
    status: "True"
    type: Ready
  - lastProbeTime: null
    lastTransitionTime: 2018-04-19T03:25:57Z
    status: "True"
    type: PodScheduled
  containerStatuses:
  - containerID: hyper://156152bee0b70371a5dd2dc9e6e59b31d8aca75617d75b31121522c6266e2805
    image: sha256:5195076672a7e30525705a18f7d352c920bbd07a5ae72b30e374081fe660a011
    imageID: sha256:5195076672a7e30525705a18f7d352c920bbd07a5ae72b30e374081fe660a011
    lastState: {}
    name: mysql
    ready: true
    restartCount: 0
    state:
      running:
        startedAt: 2018-04-19T03:26:03Z
  phase: Running
  podIP: 10.244.140.236
  qosClass: Burstable
  startTime: 2018-04-19T03:25:57Z
```
> `volumeID` is the name of `volume`



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
```


delete
------------------------------
Delete one or more pod(s)

    Usage: pi delete pods POD [OPTIONS]

    Delete a pod

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username

```sh
$ pi delete pods busybox
pod "busybox" deleted
```
