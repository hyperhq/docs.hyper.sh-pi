# Floating IP

A floating IP (aka _FIP_), is a public accessible IPv4 address. It is used with [_Service_](../Feature/service.md) to enable public access to the associated pods. You cannot attach multiple floating IPs to a _service_, but you can associate multiple services with one single floating IP (as long as no port flicts among different services).

If you allocate a floating IP address but do not use it, you will be [charged for the IP address](../Overview/pricing.md#Network). If you use it (with _Service_), you will not be charged for it.

Lifecycle
---------------------------

You need to explicitly allocate and release floating IPs:

```sh
$ pi create fip
fip/104.197.12.87

$ pi delete fip 104.197.12.87
fip "104.197.12.87" deleted
```

You cannot release an FIP while it is in use.

Associate Floating IP with Service
---------------------------

Create a new service using an existing floating IP:

```sh
$ pi create fip
fip/104.154.140.179

$ cat <<EOF > /tmp/httpLB.yml
apiVersion: v1
kind: Service
metadata:
  name: http-lb
spec:
  type: LoadBalancer
  loadBalancerIP: 104.154.140.179
  selector:
    app: http
  ports:
    - port: 80
      targetPort: 8080
EOF
$ pi create -f /tmp/httpLB.yml
service/http-lb
```

You can also associate multiple service to one floating IP, with different ports:

```sh
$ cat <<EOF > /tmp/httpsLB.yml
apiVersion: v1
kind: Service
metadata:
  name: https-lb
spec:
  type: LoadBalancer
  loadBalancerIP: 104.154.140.179
  selector:
    app: https
  ports:
    - port: 443
      targetPort: 4433
EOF
$ pi create -f /tmp/httpsLB.yml
service/https-lb
```

Billing
---------------------------

If you allocate a floating IP address but do not use it, you will be charged with an hourly rate ($0.01/hour). If you use it (with _Service_), you will not be charged for it.
