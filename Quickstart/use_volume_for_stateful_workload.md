# Use volume for stateful workload

In [the previous chapter](../Quickstart/understand_network.md) we learned how to setup port mappings to enable public access to pods. Now we'll look at how to persist data using [_Volumes_](../Feature/volume.md).

First you'll need to create a volume:

```sh
$ pi create volume data --zone gcp-us-central1-a --size 10
volume/data
```

Next, we will launch a busybox pod with this volume:

```sh
$ cat <<EOF > /tmp/busybox.yml
apiVersion: v1
kind: Pod
metadata:
  name: busybox
spec:
  containers:
  - name: busybox
    image: busybox
    volumeMounts:
    - name: data
      mountPath: /data
  nodeSelector:
    zone: gcp-us-central1-a
  volumes:
  - name: data
    flexVolume:
      options:
        volumeID: "data"
EOF
$ pi create -f /tmp/busybox.yml
pod/busybox
```

Now, let's create a file on the volume:

```sh
$ pi exec busybox -c busybox -- sh -c 'echo "Hello World" > /data/hello.txt'
$ pi exec busybox -c busybox -- cat /data/hello.txt
Hello World
```

To demonstrate the data persistence, we'll delete the pod, and create another a new one with the same volume to see whether the file is still there:

```sh
$ pi delete pod busybox --now
pod "busybox" deleted

$ pi create -f /tmp/busybox.yml
pod/busybox

$ pi exec busybox -c busybox -- cat /data/hello.txt
Hello World
```

For stateful applications, we suggest you to use volumes for persistent data. Even in the case of long-running batch jobs, such as data mining, machine learning, keep syncing the in-transit results to volumes is a best practice for fault tolerance.

>**Note on volume mount**
> In the cases of pod recovery (recreated due to HW/SW errors, rescheduling, etc.), the new pod (same pod name, different pod id) will have identical mountpoints, which means that the same volumes will be automatically mounted onto the new pod. There is no action needed on your part.

### Cleanup

Volumes are indepedent resources, and only removable when they are not mounted.

```sh
$ pi delete pod busybox
pod "busybox" deleted

$ pi delete volume data
volume "data" deleted
```

Volumes are billed per second. Therefore, it makes sense to delete them as soon as possible to save cost.

In [the final section](../Quickstart/working_with_multi-container_pod.md) we'll see how multi-container pod works.
