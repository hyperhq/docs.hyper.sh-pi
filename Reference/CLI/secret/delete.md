delete
------------------------------
Delete one or more secret(s)

    Usage: pi delete secrets SECRET [OPTIONS]

    Delete a secret

              -e, --access-key                API access key
              -k, --secret-key                API secret key
              -r, --region                    Region name, default: gcp-us-central1
              -u, --user                      Username
              --all                           Delete all resources, including uninitialized ones, in the namespace of the specified resource types.
```sh
$ pi delete secrets test-secret-gitlab
secret "test-secret-gitlab" deleted
```
