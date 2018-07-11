# Job - Run to Completion

A job creates one or more pods and ensures that a specified number of them successfully terminate. As pods successfully complete, the job tracks the successful completions. When a specified number of successful completions is reached, the job itself is complete. Deleting a Job will cleanup the pods it created.

A simple case is to create one Job object in order to reliably run one Pod to completion. The Job object will start a new Pod if the first pod fails or is deleted (for example due to a node hardware failure or a node reboot).

A Job can also be used to run multiple pods in parallel.

## Running an example Job

Here is an example Job config.  It computes π to 2000 places and prints it out.
It takes around 10s to complete.

```shell
$ pi create job pi --image=pi --restart=Never --backoff-limit=4 -- perl -Mbignum=bpi -wle "print bpi(2000)"
job "pi" created
```

Check on the status of the job using this command:

```shell
$ pi describe jobs/pi
Name:             pi
Namespace:        default
Selector:         controller-uid=b1db589a-2c8d-11e6-b324-0209dc45a495
Labels:           controller-uid=b1db589a-2c8d-11e6-b324-0209dc45a495
                  job-name=pi
Annotations:      <none>
Parallelism:      1
Completions:      1
Start Time:       Tue, 03 Jul 2018 15:02:53 +0800
Pods Statuses:    0 Running / 1 Succeeded / 0 Failed
Pod Template:
  Labels:       controller-uid=b1db589a-2c8d-11e6-b324-0209dc45a495
                job-name=pi
  Containers:
   pi:
    Image:      perl
    Port:
    Command:
      perl
      -Mbignum=bpi
      -wle
      print bpi(2000)
    Environment:        <none>
    Mounts:             <none>
  Volumes:              <none>
Events:
  FirstSeen    LastSeen    Count    From            SubobjectPath    Type        Reason            Message
  ---------    --------    -----    ----            -------------    --------    ------            -------
  1m           1m          1        {job-controller }                Normal      SuccessfulCreate  Created pod: pi-dtn4q
```

To view completed pods of a job, use `pi get pods`.

To list all the pods that belong to a job in a machine readable form, you can use a command like this:

```shell
$ pods=$(pi get pods --selector=job-name=pi --output=jsonpath={.items..metadata.name})
$ echo $pods
pi-aiw0a
```

Here, the selector is the same as the selector for the job.  The `--output=jsonpath` option specifies an expression that just gets the name from each pod in the returned list.

View the standard output of one of the pods:

```shell
$ pi logs $pods
3.1415926535897932384626433832795028841971693993751058209749445923078164062862089986280348253421170679821480865132823066470938446095505822317253594081284811174502841027019385211055596446229489549303819644288109756659334461284756482337867831652712019091456485669234603486104543266482133936072602491412737245870066063155881748815209209628292540917153643678925903600113305305488204665213841469519415116094330572703657595919530921861173819326117931051185480744623799627495673518857527248912279381830119491298336733624406566430860213949463952247371907021798609437027705392171762931767523846748184676694051320005681271452635608277857713427577896091736371787214684409012249534301465495853710507922796892589235420199561121290219608640344181598136297747713099605187072113499999983729780499510597317328160963185950244594553469083026425223082533446850352619311881710100031378387528865875332083814206171776691473035982534904287554687311595628638823537875937519577818577805321712268066130019278766111959092164201989380952572010654858632788659361533818279682303019520353018529689957736225994138912497217752834791315155748572424541506959508295331168617278558890750983817546374649393192550604009277016711390098488240128583616035637076601047101819429555961989467678374494482553797747268471040475346462080466842590694912933136770289891521047521620569660240580381501935112533824300355876402474964732639141992726042699227967823547816360093417216412199245863150302861829745557067498385054945885869269956909272107975093029553211653449872027559602364806654991198818347977535663698074265425278625518184175746728909777727938000816470600161452491921732172147723501414419735685481613611573525521334757418494684385233239073941433345477624168625189835694855620992192221842725502542568876717904946016534668049886272327917860857843838279679766814541009538837863609506800642251252051173929848960841284886269456042419652850222106611863067442786220391949450471237137869609563643719172874677646575739624138908658326459958133904780275901
```

### Parallel Jobs

There are three main types of jobs:

1. Non-parallel Jobs
  - normally only one pod is started, unless the pod fails.
  - job is complete as soon as Pod terminates successfully.
1. Parallel Jobs with a *fixed completion count*:
  - specify a non-zero positive value for `completions`.
  - the job is complete when there is one successful pod for each value in the range 1 to `completions`.
  - **not implemented yet:** Each pod passed a different index in the range 1 to `completions`.
1. Parallel Jobs with a *work queue*:
  - do not specify `completions`, default to `parallelism`.
  - the pods must coordinate with themselves or an external service to determine what each should work on.
  - each pod is independently capable of determining whether or not all its peers are done, thus the entire Job is done.
  - when _any_ pod terminates with success, no new pods are created.
  - once at least one pod has terminated with success and all pods are terminated, then the job is completed with success.
  - once any pod has exited with success, no other pod should still be doing any work or writing any output.  They should all be
    in the process of exiting.

For a Non-parallel job, you can leave both `completions` and `parallelism` unset.  When both are unset, both are defaulted to 1.

For a Fixed Completion Count job, you should set `completions` to the number of completions needed. You can set `parallelism`, or leave it unset and it will default to 1.

For a Work Queue Job, you must leave `completions` unset, and set `parallelism` to
a non-negative integer.

For more information about how to make use of the different types of job, see the [job patterns](#job-patterns) section.


#### Controlling Parallelism

The requested parallelism (`parallelism`) can be set to any non-negative value.
If it is unspecified, it defaults to 1. If it is specified as 0, then the Job is effectively paused until it is increased.

Actual parallelism (number of pods running at any instant) may be more or less than requested parallelism, for a variety of reasons:

- For Fixed Completion Count jobs, the actual number of pods running in parallel will not exceed the number of remaining completions.   Higher values of `parallelism` are effectively ignored.
- For work queue jobs, no new pods are started after any pod has succeeded -- remaining pods are allowed to complete, however.
- If the controller has not had time to react.
- If the controller failed to create pods for any reason (lack of ResourceQuota, lack of permission, etc.), then there may be fewer pods than requested.
- The controller may throttle new pod creation due to excessive previous pod failures in the same Job.
- When a pod is gracefully shutdown, it takes time to stop.

## Handling Pod and Container Failures

A Container in a Pod may fail for a number of reasons, such as because the process in it exited with a non-zero exit code, or the Container was killed for exceeding a memory limit, etc.  If this happens, and the `restart = "OnFailure"`, then the Container is re-run.  Therefore, your program needs to handle the case when it is restarted locally, or else specify `restart = "Never"`.

An entire Pod can also fail, for a number of reasons, such as when the pod is kicked off, or if a container of the Pod fails and the `restart = "Never"`.  When a Pod fails, then the Job controller starts a new Pod.  Therefore, your program needs to handle the case when it is restarted in a new pod.  In particular, it needs to handle temporary files, locks, incomplete output and the like caused by previous runs.

Note that even if you specify `parallelism = 1` and `completions = 1` and `restart = "Never"`, the same program may sometimes be started twice.

If you do specify `parallelism` and `completions` both greater than 1, then there may be
multiple pods running at once.  Therefore, your pods must also be tolerant of concurrency.

### Pod Backoff failure policy

There are situations where you want to fail a Job after some amount of retries due to a logical error in configuration etc. To do so, set `backoffLimit` to specify the number of retries before considering a Job as failed. The back-off limit is set by default to 6. Failed Pods associated with the Job are recreated by the Job controller with an
exponential back-off delay (10s, 20s, 40s ...) capped at six minutes. The back-off count is reset if no new failed Pods appear before the Job's next status check.

**Note:** Due to a known issue [#54870](https://github.com/kubernetes/kubernetes/issues/54870), when the `restart` field is set to "`OnFailure`", the back-off limit may be ineffective. As a short-term workaround, set the restart policy for the embedded template to "`Never`".

## Job Termination and Cleanup

When a Job completes, no more Pods are created, but the Pods are not deleted either.  Keeping them around allows you to still view the logs of completed pods to check for errors, warnings, or other diagnostic output. The job object also remains after it is completed so that you can view its status.  It is up to the user to delete
old jobs after noting their status.  Delete the job with `pi` (e.g. `pi delete jobs/pi` ). When you delete the job using `pi`, all the pods it created are deleted too.

By default, a Job will run uninterrupted unless a Pod fails, at which point the Job defers to the `backoffLimit` described above. Another way to terminate a Job is by setting an active deadline. Do this by setting the `activeDeadlineSeconds` field of the Job to a number of seconds.

The `activeDeadlineSeconds` applies to the duration of the job, no matter how many Pods are created. Once a Job reaches `activeDeadlineSeconds`, the Job and all of its Pods are terminated. The result is that the job has a status with `reason: DeadlineExceeded`.

Note that a Job's `activeDeadlineSeconds` takes precedence over its `backoffLimit`. Therefore, a Job that is retrying one or more failed Pods will not deploy additional Pods once it reaches the time limit specified by `activeDeadlineSeconds`, even if the `backoffLimit` is not yet reached.

## Job Patterns

The Job object can be used to support reliable parallel execution of Pods.  The Job object is not designed to support closely-communicating parallel processes, as commonly found in scientific computing.  It does support parallel processing of a set of independent but related *work items*.
These might be emails to be sent, frames to be rendered, files to be transcoded, ranges of keys in a NoSQL database to scan, and so on.

In a complex system, there may be multiple different sets of work items.  Here we are just considering one set of work items that the user wants to manage together &mdash; a *batch job*.

There are several different patterns for parallel computation, each with strengths and weaknesses. The tradeoffs are:

- One Job object for each work item, vs. a single Job object for all work items.  The latter is better for large numbers of work items.  The former creates some overhead for the user and for the system to manage large numbers of Job objects.
- Number of pods created equals number of work items, vs. each pod can process multiple work items. The former typically requires less modification to existing code and containers. The latter is better for large numbers of work items, for similar reasons to the previous bullet.
- Several approaches use a work queue.  This requires running a queue service, and modifications to the existing program or container to make it use the work queue. Other approaches are easier to adapt to an existing containerised application.

The tradeoffs are summarized here, with columns 2 to 4 corresponding to the above tradeoffs. The pattern names are also links to examples and more detailed description.

|                            Pattern                                   | Single Job object | Fewer pods than work items? | Use app unmodified? |  Works in Kube 1.1? |
| -------------------------------------------------------------------- |:-----------------:|:---------------------------:|:-------------------:|:-------------------:|
| Job Template Expansion           |                   |                             |          ✓          |          ✓          |
| Queue with Pod Per Work Item   |         ✓         |                             |      sometimes      |          ✓          |
| Queue with Variable Pod Count|         ✓         |             ✓               |                     |          ✓          |
| Single Job with Static Work Assignment                               |         ✓         |                             |          ✓          |                     |

When you specify completions with `completions`, each Pod created by the Job controller
has an identical [`spec`](https://git.k8s.io/community/contributors/devel/api-conventions.md#spec-and-status).  This means that all pods will have the same command line and the same image, the same volumes, and (almost) the same environment variables.  These patterns are different ways to arrange for pods to work on different things.

This table shows the required settings for `parallelism` and `completions` for each of the patterns.Here, `W` is the number of work items.

|                             Pattern                                  | `completions` |  `parallelism` |
| -------------------------------------------------------------------- |:-------------------:|:--------------------:|
| [Job Template Expansion](/docs/tasks/job/parallel-processing-expansion/)           |          1          |     should be 1      |
| [Queue with Pod Per Work Item](/docs/tasks/job/coarse-parallel-processing-work-queue/)   |          W          |        any           |
| [Queue with Variable Pod Count](/docs/tasks/job/fine-parallel-processing-work-queue/)  |          1          |        any           |
| Single Job with Static Work Assignment                               |          W          |        any           |
