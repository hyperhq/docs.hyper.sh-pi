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
$ pi describe jobs testjob
TODO
```
