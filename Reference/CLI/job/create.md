create
------------------------------
Create new pod

    Usage: pi create job JOB --image [OPTIONS] -- [COMMAND] [args...]

    Create a new job

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username
              --image-pull-secrets            Docker registry secret to pull the image, not required for public repo
              --size                          Pod size, default: S4
              --env                           Array of environment variables to set in the pods of the job
              --labels                        Array of labels to tag to the pods of the job
              --restart                       Pod restart policy: OnFailure/Never. “OnFailure” tries to restart failed container until “backoffLimit” is reached. `Never` never attemps to restart the failed container
              --active-deadline-seconds       The duration of the job, no matter how many Pods are created. Once a Job reaches activeDeadlineSeconds, the Job and all of its Pods are terminated. The result is that the job has a status with reason: DeadlineExceeded
              --completions                   The job is complete when there is one successful pod for each value in the range 1 to "completions", default=1
              --parallelism                   The concurrent pods to run for a job, default=1
              --backoff-limit                 There are situations where you want to fail a Job after some amount of retries due to a logical error in configuration etc. To do so, set "backoffLimit" to specify the number of retries before considering a Job as failed. The back-off limit is set by default to 6. Failed Pods associated with the Job are recreated by the Job controller with an exponential back-off delay (10s, 20s, 40s …) capped at six minutes. The back-off count is reset if no new failed Pods appear before the Job’s next status check.

The `create job` command submits the job request to _Pi_.

```sh
$ pi create job transcoder --image=transcoder /opt/run.sh https://s3.us-east-2.amazonaws.com/bucketname/pi_/demo.mp4
job/transcoder
```
