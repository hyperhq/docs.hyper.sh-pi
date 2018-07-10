get
------------------------------
List jobs or get a job

    Usage: pi get jobs JOB [OPTIONS]

    Get a job's detailed information

          -e, --access-key                API access key
          -k, --secret-key                API secret key
          -r, --region                    Region name, default: gcp-us-central1
          -u, --user                      Username
          -o, --output                    Output format (json, yaml, name, wide)
          -l, --selector=''               Selector (label query) to filter on, supports '=', '==', and '!='.(e.g. -l key1=value1,key2=value2)
          -a, --show-all=false            When printing, show all resources (default hide terminated pods.)


```sh
//list jobs
$ pi get jobs
NAME      DESIRED   SUCCESSFUL   AGE
job1      1         0            5m
job2      1         0            1m

//get a pod
$ pi get jobs job1
NAME      DESIRED   SUCCESSFUL   AGE
job1      1         0            6m
```
