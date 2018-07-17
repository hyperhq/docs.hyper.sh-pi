Pull an Image from a Private Registry
----------------------------------------------------------

# Steps to use private image

1. get account from image registry
2. create secret with the above account
3. create pod with the above secret

# Examples

Here are some examples to run pod with private image.

## Docker Hub

```
$ pi create secret docker-registry regcred-dockerhub \
  --docker-username=xjimmyshcn \
  --docker-password='xxxxxxxxxx' \
  --docker-email=xxxxxxxxxx


$ cat pod-dockerhub.yaml
apiVersion: v1
kind: Pod
metadata:
  name: alpine-dockerhub
spec:
  containers:
  - name: alpine
    image: xjimmyshcn/alpine:latest
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  imagePullSecrets:
  - name: regcred-dockerhub


//use spec.imagePullSecrets
$ pi create -f pod-dockerhub.yaml

or

//use --image-pull-secrets
$ pi create pod alpine-dockerhub --image=xjimmyshcn/alpine:latest \
  --image-pull-secrets=regcred-dockerhub \
  -- sh -c 'echo The app is running! && sleep 3600'
```


## Gitlab

```
$ pi create secret docker-registry regcred-gitlab \
  --docker-server=registry.gitlab.com \
  --docker-email=xxxxxxxxxx \
  --docker-username=xjimmy \
  --docker-password='xxxxxxxxxxxx'

//create pod with secret regcred-gitlab
```


## GCR

```
//get the keyfile.json for GCR, here is an example
$ cat /tmp/keyfile.json
{
  "type": "service_account",
  "project_id": "io",
  "private_key_id": "xxxxx",
  "private_key": "-----BEGIN PRIVATE KEY-----\n.........\n-----END PRIVATE KEY-----",
  "client_email": "xxxxx@xxxxx.iam.gserviceaccount.com",
  "client_id": "xxxxx",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://accounts.google.com/o/oauth2/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/xxxxx%40xxxxx.iam.gserviceaccount.com"
}

$ pi create secret docker-registry regcred-gcr \
  --docker-server="https://gcr.io"
  --docker-email=none \
  --docker-username=_json_key \
  --docker-password="$(cat /tmp/keyfile.json)"


//create pod with secret regcred-gcr
```


## ECR

```
//get user account for ECR
$ aws ecr get-login --no-include-email --region ap-southeast-1
docker login -u AWS -p eyJ...OX0= https://xxxxx.dkr.ecr.ap-southeast-1.amazonaws.com

$ ECR_PASSWD="eyJ...OX0= https://xxxxx.dkr.ecr.ap-southeast-1.amazonaws.com"
$ pi create secret docker-registry regcred-ecr \
  --docker-server=https://xxxxx.dkr.ecr.ap-southeast-1.amazonaws.com \
  --docker-email=none
  --docker-username=AWS \
  --docker-password='$ECR_PASSWD' \


//create pod with secret regcred-ecr
```
