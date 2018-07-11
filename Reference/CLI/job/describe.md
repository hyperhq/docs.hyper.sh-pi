describe
------------------------------
Show details of job

    Usage: pi describe jobs JOD [OPTIONS]

    Get a job's detailed description, including related events.

          -e, --access-key                API access key
          -k, --secret-key                API secret key
          -r, --region                    Region name, default: gcp-us-central1
          -u, --user                      Username
          -l, --selector=''               Selector (label query) to filter on, supports '=', '==', and '!='.(e.g. -l key1=value1,key2=value2)
          --include-uninitialized         If true, the pi command applies to uninitialized objects.

```sh
$ pi describe jobs pi
Name:           pi
Namespace:      default
Selector:       controller-uid=53d05fa3-8330-11e8-b8a4-42010a000032
Labels:         controller-uid=53d05fa3-8330-11e8-b8a4-42010a000032
                job-name=pi
Annotations:    id=e4deba464dfb3d0d7380ccaf6e7d48340ee1122efce7f503f8c724aac759a017
                sh_hyper_instancetype=s4
                sh_hyper_is_extra=false
                zone=gcp-us-central1-b
Parallelism:    1
Completions:    1
Start Time:     Mon, 09 Jul 2018 12:27:02 +0800
Pods Statuses:  0 Running / 1 Succeeded / 0 Failed
Pod Template:
  Labels:       controller-uid=53d05fa3-8330-11e8-b8a4-42010a000032
                job-name=pi
  Annotations:  id=e4deba464dfb3d0d7380ccaf6e7d48340ee1122efce7f503f8c724aac759a017
                sh_hyper_instancetype=s4
                sh_hyper_is_extra=false
                zone=gcp-us-central1-b
  Containers:
   pi:
    Image:  perl
    Port:   <none>
    Command:
      perl
      -Mbignum=bpi
      -wle
      print bpi(2000)
    Limits:
      cpu:     1
      memory:  512Mi
    Requests:
      cpu:        31m
      memory:     512Mi
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Events:
  Type    Reason            Age   From            Message
  ----    ------            ----  ----            -------
  Normal  SuccessfulCreate  5m    job-controller  Created pod: pi-f8jk9
```
