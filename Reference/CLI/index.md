# CLI

> **Note**: _pi_ is based on [`kubectl`](https://v1-9.docs.kubernetes.io/docs/reference/kubectl/overview/).

`pi` is a command line interface for running commands against _Pi_ cloud. This overview covers `pi` syntax, describes the command operations, and provides common examples.

## Install

We currently provides official CLI builds for Linux and Mac.

- Linux x86
```sh
$ wget https://hyper-install.s3.amazonaws.com/pi-linux-x86_64.tar.gz
$ tar xzf pi-linux-x86_64.tar.gz
$ ./pi
```

- Mac OS X 10.7 (lion) or later
```
$ curl -O https://hyper-install.s3.amazonaws.com/pi-mac.bin.zip
$ unzip pi-mac.bin.zip
$ ./pi
```

## Configure pi

In order for pi to find and access a Pi regiion, it needs a configuration file, which, by default, is located at `~/.pi/config`.

```sh
//set credentials for user1(alias)
$ pi config set-credentials user1 --region=gcp-us-central1 --access-key="xxx" --secret-key="xxxxxx"

//set another credentials for user2(alias, default region is "gcp-us-central1" )
$ pi config set-credentials user2 --access-key="yyy" --secret-key="yyyyyy"

//select default user
$ pi config set-context default --user=user2

//delete credentials for user1
$ pi config delete-credentials user1

//view config
$ pi config view
apiVersion: v1
clusters:
- cluster:
    insecure-skip-tls-verify: true
    server: https://*.hyper.sh:443
  name: default
contexts:
- context:
    cluster: default
    namespace: default
    user: user2
  name: default
current-context: default
kind: Config
preferences: {}
users:
- name: user2
  user:
    access-key: yyy
    region: gcp-us-central1
    secret-key: yyyyyy


// config file:
$ cat ~/.pi/config
```

## Operations

> Find more information at https://github.com/hyperhq/pi.

#### Basic Commands (Beginner):

- `create`      Create a resource.

#### Basic Commands (Intermediate):

- `get`         Display one or many resources
- `delete`      Delete resources by resources and names
- `name`        Name a resource

#### Troubleshooting and Debugging Commands:
- `exec`        Execute a command in a container

#### Other Commands:
- `config`      Modify pi config files
- `help`        Help about any command
- `info`        Print region and user info

Usage:
  pi [flags] [options]

Use "pi <command> --help" for more information about a given command.
Use "pi options" for a list of global command-line options (applies to all commands).
