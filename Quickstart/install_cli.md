# Install CLI

## Generate API Credential

First of all, you need to generate the API credential:

- Go to the [account page](https://console.hyper.sh/account/credential) in the console
- Click `Create Credential`

![](https://trello-attachments.s3.amazonaws.com/5700ea0da7030dcf7485ed70/57ac415d5c5774e392d184a5/9e0252c0ea100f159e8c943316dbf8b9/credential.png)

- A new credential will be created:

![](https://trello-attachments.s3.amazonaws.com/5700ea0da7030dcf7485ed70/57ac415d5c5774e392d184a5/ef14d78bad84d179bcf460226ca3f075/new-credential.png)

> NOTE: please download the secret key immediately. Once the popup is closed, there is no way to find the secret key.

## Install CLI

We currently provides official CLI builds for Linux and Mac.

- Linux x86_64
```sh
$ wget https://github.com/hyperhq/pi/releases/download/latest/pi.linux-amd64.tar.gz
$ tar xzf pi.linux-amd64.tar.gz
$ ./pi help
```

- Mac OS X 10.7 (lion) or later
```
$ curl -OL# https://github.com/hyperhq/pi/releases/download/latest/pi.darwin-amd64.zip
$ unzip pi.darwin-amd64.zip
$ ./pi help
```

> or download binary from https://github.com/hyperhq/pi/releases

## Configure pi

In order for the CLI to find and access a region, it needs a configuration file, which, by default, is located at `~/.pi/config`.

```sh
//set user1 credential
$ pi config set-credentials user1 --region=gcp-us-central1 --access-key="xxx" --secret-key="xxxxxx"

//set user2 credentials
$ pi config set-credentials user2 --region=gcp-us-central1 --access-key="xxx" --secret-key="xxxxxx"

//select default user
$ pi config set-context default --user=user2

//view config file:
$ cat ~/.pi/config
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
- name: user1
  user:
    access-key: xxx
    region: gcp-us-central1
    secret-key: xxxxxx
- name: users
  user:
    access-key: xxx
    region: gcp-us-central1
    secret-key: xxxxxx
```

## Check the pi configuration
Check that pi is properly configured by getting the region information:

```shell
$ pi info
Region Info:
  Region                 gcp-us-central1
  AvailabilityZone       gcp-us-central1-a|UP
  ServiceClusterIPRange  10.96.0.0/12
Account Info:
  Email                  user1@test.com
  TenantID               00a54ebcc0444bb384e48f6fd7b5597b
  DefaultZone            gcp-us-central1-a
  Resources              pod:1/20,volume:1/40,fip:1/5,service:4/5,secret:1/3
Version Info:
  Version                alpha-0.4
  Hash                   f73941d7
  Build                  2018-04-19T19:11:54+0800
```
