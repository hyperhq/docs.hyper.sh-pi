logs
------------------------------
Print the logs for a container in a pod.

    Usage: pi logs POD [OPTIONS]

    Get a pod's logs

          -e, --access-key                API access key
          -k, --secret-key                API secret key
          -r, --region                    Region name, default: gcp-us-central1
          -u, --user                      Username
          -c, --container                 Print the logs of this container
          --limit-bytes                   Maximum bytes of logs to return. Defaults to no limit.
          --pod-running-timeout           The length of time (like 5s, 2m, or 3h, higher than zero) to wait until at least one pod is running
          -p, --previous                  If true, print the logs for the previous instance of the container in a pod if it exists.
          -l, --selector                  Selector (label query) to filter on.
          --since                         Only return logs newer than a relative duration like 5s, 2m, or 3h. Defaults to all logs. Only one of since-time / since may be used.
          --since-time                    Only return logs after a specific date (RFC3339). Defaults to all logs. Only one of since-time / since may be used.
          --tail                          Lines of recent log file to display. Defaults to -1 with no selector, showing all log lines otherwise 10, if a selector is provided.
          --timestamps                    Include timestamps on each line in the log output


```sh
$ pi logs mongo -c mongo --tail=1
2018-04-24T01:38:21.126+0000 I COMMAND  [thread1] command config.$cmd command: createIndexes { createIndexes: "system.sessions", indexes: [ { key: { lastUse: 1 }, name: "lsidTTLIndex", expireAfterSeconds: 1800 } ], $db: "config" } numYields:0 reslen:98 locks:{ Global: { acquireCount: { r: 1, w: 1 } }, Database: { acquireCount: { W: 1 } }, Collection: { acquireCount: { w: 1 } } } protocol:op_msg 320ms
```
