# Working with multi-container pod

So far, we've learned to use single-container pod. This chapter will show you how to use multi-container pods.

In _Pi_, containers in the same pod share [Linux namespaces](https://en.wikipedia.org/wiki/Linux_namespaces), e.g.

- Process ID (pid)
- Network (net)
- Interprocess Communication (ipc)
- UTS
- User ID (user)

For example, if processes in multiple containers try to bind the same port, only the first attempt will succeed and the rest will fail. However, each container comes with its down _rootfs_, which means that different containers have different filesystem and mountpoints. To see this, let's start with a basic multi-container pod:

```sh
$ cat <<EOF > /tmp/multi-basic.yml
apiVersion: v1
kind: Pod
metadata:
  name: multi-basic
spec:
  containers:
  - name: busybox
    image: busybox
  - name: nginx
    image: nginx
EOF
$ pi create -f /tmp/multi-basic.yml
pod/multi-basic
```

You can access a particular container by specifying the container name in the `pod exec` command:

```sh
$ pi exec -it multi-basic --container busybox -- /bin/sh
$ pi exec -it multi-basic -c nginx -- /bin/sh
```

> Note:
> If _--container_ is absent, the first container in the pod spec will be the default container to execute the command.

Now, let's these two containers' filesystem:

```sh
$ pi exec multi-basic --container busybox -- file /etc/nginx.conf
/etc/nginx.conf: cannot open '/etc/nginx.conf' (No such file or directory)

$ pi exec multi-basic --container nginx -- file /etc/nginx.conf
/etc/nginx/nginx.conf: ASCII English text
```
