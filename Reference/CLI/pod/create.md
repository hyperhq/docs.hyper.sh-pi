create
------------------------------
Create new pod

    Usage: pi create pod [OPTIONS]

    Create a new pod

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username
              --active-deadline-seconds       Optional duration in seconds the pod may be active on the node relative to StartTime before the system will actively try to mark it failed and kill associated containers. Value must be a positive integer.
              --env                           Environment variables to set in the container
              --image                         The image for the container to run.
              --image-pull-secrets            The secret for the private docker registry, comma separated.
              -l, --labels                    Comma separated labels to apply to the pod(s). Will override previous values.
              --limits                        The resource requirement limits for this container. For example, 'cpu=200m,memory=512Mi'.
              --restart                       The restart policy for this Pod. Default 'Always'
              --rm                            If true, delete resources created in this command for attached containers.
              --size                          The size for the pod (e.g. s1, s2, s3, s4, m1, m2, m3, l1, l2, l3, l4, l5, l6). Default 's4'
              -i, --stdin                     Keep stdin open on the container(s) in the pod, even if nothing is attached.
              -t, --tty                       Allocated a TTY for each container in the pod.
              --volume                        Pod volumes to mount into the container's filesystem.

The `create` command submits the pod request specified in the pod spec file to _Pi_.

```sh
  # Start a single instance of nginx.
  pi create pod nginx --image=nginx

  # Start a single instance of nginx and set environment variables "DNS_DOMAIN=cluster" and "POD_NAMESPACE=default" in the container.
  pi create pod nginx --image=nginx --env="DNS_DOMAIN=cluster" --env="POD_NAMESPACE=default"

  # Start a single instance of nginx and set labels "app=nginx" and "env=prod" in the container.
  pi create pod nginx --image=nginx --labels="app=nginx,env=prod"

  # Start a pod of busybox and keep it in the foreground, don't restart it if it exits.
  pi create pod -it busybox --image=busybox --restart=Never -- sh

  # Start the nginx container using a specified command and custom arguments.
  pi create pod nginx --image=nginx -- <cmd> <arg1> ... <argN>

  # Start the nginx container using a specified command and custom arguments.
  pi create pod nginx --rm --image=nginx -- echo hello world
```
