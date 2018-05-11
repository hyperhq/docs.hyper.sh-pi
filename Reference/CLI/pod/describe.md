describe
------------------------------
Show details of pod

    Usage: pi describe pods POD [OPTIONS]

    Get a pod's detailed description, including related events.

          -e, --access-key                API access key
          -k, --secret-key                API secret key
          -r, --region                    Region name, default: gcp-us-central1
          -u, --user                      Username
          -l, --selector=''               Selector (label query) to filter on, supports '=', '==', and '!='.(e.g. -l key1=value1,key2=value2)
          --include-uninitialized         If true, the pi command applies to uninitialized objects.

```sh
$ pi describe pods mongo
Name:         mongo
Namespace:    default
Node:         <none>
Start Time:   Wed, 02 May 2018 20:38:04 +0800
Labels:       run=mongo
Annotations:  id=231823bcbb89ae0f0175655cf9d5995730fc82821470cb745b9c521196ab3a09
              sh_hyper_instancetype=s4
              zone=gcp-us-central1-a
Status:       Pending
IP:
Containers:
  mongo:
    Container ID:
    Image:          mongo
    Image ID:
    Port:           <none>
    State:          Waiting
      Reason:       ContainerCreating
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:         <none>
Conditions:
  Type           Status
  Initialized    True
  Ready          False
  PodScheduled   True
Volumes:         <none>
QoS Class:       Burstable
Node-Selectors:  <none>
Tolerations:     <none>
Events:
  Type    Reason                 Age   From               Message
  ----    ------                 ----  ----               -------
  Normal  Scheduled              37s   default-scheduler  Successfully assigned mongo to compute node a-4
  Normal  PortCreated            37s   pod-watcher        Successfully created port "pod-mongo"
  Normal  SuccessfulMountVolume  36s   kubelet, a-4       MountVolume.SetUp succeeded for volume "default-token-w5pf4"
  Normal  PortSynced             36s   port-controller    Successfully synced port "pod-mongo"
  Normal  Pulling                35s   kubelet, a-4       pulling image "mongo"
  Normal  Pulled                 1s    kubelet, a-4       Successfully pulled image "mongo"
```
