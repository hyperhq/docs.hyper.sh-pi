# Info

Check region and account info

```shell
$ pi info
Region Info:
  Region                 gcp-us-central1
  AvailabilityZone       gcp-us-central1-b|UP
  ServiceClusterIPRange  10.96.0.0/12
Account Info:
  Email                  testuser3@test.com
  TenantID               00a54ebcc0444bb384e48f6fd7b5597b
  DefaultZone            gcp-us-central1-b
  Resources              pod:1/20,volume:1/40,fip:1/5,service:4/5,secret:1/3
Version Info:
  Version                v1.9-b18042723
  Hash                   beb46e81
  Build                  2018-04-27T23:33:59+0800


//force to check new version of pi
$ pi info --check-update
```

- `Region` is current region name
- `AvailabilityZone` is all availability zones for user
- `ServiceClusterIPRange` is the ip range for create ClusterIP type service
- `TenantID` is the user ID
- `DefaultZone` is the default zone for user create pod and volume
- `Resources` are resources usage and quota


If you see a message similar to the following, pi is not correctly configured or not able to connect to the API Server.

```shell
Unable to connect to the server: dial tcp: lookup <server-name>: no such host
or
Unable to connect to the server: dial tcp <server-name:port>: i/o timeout
```
