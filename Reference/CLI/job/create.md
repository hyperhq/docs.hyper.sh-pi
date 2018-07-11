create
------------------------------
Create new job

## create via yaml file
    Usage: pi [OPTIONS] create -f FILENAME
  
      Create a new job

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username
              -f, --filename                  Filename, directory, or URL to files to use to create the job, support yaml or json

```
$ pi create -f job-test.yaml
job/job-test
```

## create via command line

    Usage: pi create job JOB --image [OPTIONS] -- [COMMAND] [args...]

    Create a new job

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username
              --active-deadline-seconds       The duration of the job, no matter how many Pods are created. Once a Job reaches activeDeadlineSeconds, the Job and all of its Pods are terminated. The result is that the job has a status with reason: DeadlineExceeded
              --backoff-limit                 There are situations where you want to fail a Job after some amount of retries due to a logical error in configuration etc. To do so, set "backoffLimit" to specify the number of retries before considering a Job as failed. The back-off limit is set by default to 6. Failed Pods associated with the Job are recreated by the Job controller with an exponential back-off delay (10s, 20s, 40s …) capped at six minutes. The back-off count is reset if no new failed Pods appear before the Job’s next status check.
              --command                       If true and extra arguments are present, use them as the 'command' field in the container, rather than the 'args' field which is the default.
              --completions                   The job is complete when there is one successful pod for each value in the range 1 to "completions", default=1
              --env                           Array of environment variables to set in the pods of the job
              --image                         The image for the container to run.
              --image-pull-policy             The image pull policy for the container. If left empty, this value will not be specified by the client and defaulted by the server
              --image-pull-secrets            Docker registry secret to pull the image, not required for public repo
              --labels                        Array of labels to tag to the pods of the job
              --limits                        The resource requirement limits for this container.  For example, 'cpu=200m,memory=512Mi'.  Note that server side components may assign limits depending on the server configuration, such as limit ranges.
              --parallelism                   The concurrent pods to run for a job, default=1
              --restart                       Pod restart policy: OnFailure/Never. “OnFailure” tries to restart failed container until “backoffLimit” is reached. `Never` never attemps to restart the failed container
              --size                          Pod size, default: s4
              --volume=[]                     Pod volumes to mount into the container's filesystem. format '<volname>:<path>'


The `create job` command submits the job request to _Pi_.

```sh
$ pi create job transcoder --image=transcoder /opt/run.sh https://s3.us-east-2.amazonaws.com/bucketname/pi_/demo.mp4
job "transcoder" created

//create job with specified pod size
$ pi create job job1 --image busybox --size=s1 -- echo hello

//create job with volume
$ pi create volume vol1 --size=5
volume/vol1
$ pi create job job2 --image busybox --volume=vol1:/data
job "job2" created
$ pi get volumes
NAME  ZONE               SIZE(GB)  CREATEDAT                  POD  JOB
vol1  gcp-us-central1-c  5         2018-07-10T15:53:56+00:00       job2
```
