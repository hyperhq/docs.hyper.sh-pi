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
    zone: gcp-us-central1-a
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
