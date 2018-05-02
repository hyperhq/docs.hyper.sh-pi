delete
------------------------------
Delete one or more pod(s)

    Usage: pi delete pods POD [OPTIONS]

    Delete a pod

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username
              --all                           Delete all resources, including uninitialized ones, in the namespace of the specified resource types.
              --force                         Immediate deletion of some resources may result in inconsistency or data loss and requires confirmation.
              --grace-period                  Period of time in seconds given to the resource to terminate gracefully. Ignored if negative.
              --now                           If true, resources are signaled for immediate shutdown (same as --grace-period=1).

```sh
$ pi delete pods busybox
pod "busybox" deleted
```
