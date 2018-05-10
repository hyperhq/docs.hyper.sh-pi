# Volume

In _Pi_, volume is a persistent storage service to use with Pod. It offers high availability, durability, and consistent performance needed for stateful workloads. Multiple replicas will be automatically created with each volume to protect your data from failure.

![](https://trello-attachments.s3.amazonaws.com/5700ea0da7030dcf7485ed70/5a9946fe778993901468c0e9/3b8092f3df5f47b45c57690998d91c2d/6.png)

Volumes are constrained by availability zone. You may switch volumes between pods in the same availability zone, but there is currently no way for pod to access volumes residing in different zones. Volume name is region-wise unique, e.g. no two volumes in the same region can have the same volume name. Once a volume is attached to a pod, it will be associated with the pod throughout the pod's lifecycle, which means that:

- Volumes cannot be deleted or detached when it is attached to a pod.
-  Volumes cannot be shared. There is no way tp attache a volume to multiple pods simultaneously.
- Volumes will be automatically detached, when the associated pod terminated.

Volume use the `EXT4` filesystem. A volume size must be integer, and between 1 and 50 (GB).

The max volume number a single pod can support is 4, though one volume can be mounted by all containers in the pod (with different mountpoints). In this case, the volume is shared by all containers in that pod, however volume cannot be shared by multiple pods.

Lifecycle
---------------------------

Unlike _Rootfs_, volumes are independent from pods. You need to explicitly create and delete volumes:

```sh
$ pi create volume vol1 --size=1 --zone=gcp-us-central1-a
{
  "name": "vol1",
  "size": 1,
  "zone": "gcp-us-central1-a",
  "pod": "",
  "createdAt": "2018-03-12T03:05:45.764086754Z"
}
$ pi delete volume vol1
volume "vol1" deleted
```

To mount volumes to a container:

```sh
$ cat <<EOF > /tmp/testvol.yml
apiVersion: v1
kind: Pod
metadata:
  name: testvol
spec:
  containers:
  - name: test
    image: busybox
    volumeMounts:
    - name: data
      mountPath: /data
  volumes:
  - name: data
    flexVolume:
      options:
        volumeID: "vol1"
$ pi create -f /tmp/testvol.yml
pod/testvol
```

To verify the mount:

```sh
$ pi exec testvol -c test -- df -hT
Filesystem           Type            Size      Used Available Use% Mounted on
/dev/sdb             ext4            9.7G     37.3M      9.2G   0% /
devtmpfs             devtmpfs      242.5M         0    242.5M   0% /dev
tmpfs                tmpfs         247.1M         0    247.1M   0% /dev/shm
/dev/sda             ext4          975.9M      2.5M    906.2M   0% /data
share_dir            9p             14.6G     12.0K     14.6G   0% /var/run/secrets/kubernetes.io/serviceaccount
share_dir            9p            200.0G    132.1G     67.9G  66% /etc/hosts
share_dir            9p            200.0G    132.1G     67.9G %
```

You can also mount the same volume to different mountpoints of the same container.

```sh
$ cat <<EOF > /tmp/multi-mount.yml
apiVersion: v1
kind: Pod
metadata:
  name: multi-mount
spec:
  containers:
  - name: test1
    image: busybox
    volumeMounts:
    - name: data
      mountPath: /data
  - name: test2
    image: busybox
    volumeMounts:
    - name: data
      mountPath: /mnt/data
  volumes:
  - name: data
    flexVolume:
      options:
        volumeID: "vol1"
$ pi create -f /tmp/testvol.yml
pod/multi-mount
```

Verify

```sh
$ pi exec multi-mount -c test1 -- echo "Hello World" > /data/helloworld.txt
$ pi exec multi-mount -c test2 -- cat /mnt/data/helloworld.txt
Hello World
```

Permissions
---------------------------

Notes on reusing existing volumes: users should pay attention to volume access permissions. For example, if a volume is first mounted in pod `foo` and then reused in pod `bar`, the file/directory permissions in the volume will remain the same those set in pod `foo`. As a result, a change of container users (uid/gid pair) may effect the volume's access rights.
