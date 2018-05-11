Get
---------------------------------
get region and account info

### HTTP Request

`GET /info`

### Response

| Code | Description |
| --- | --- |
| 200 | OK |
| 500 | Server Error |

```
{
  "AvailabilityZone": "gcp-us-central1-a|UP,gcp-us-central1-c|UP",
  "DefaultZone": "gcp-us-central1-a",
  "Email": "user@test.com",
  "Region": "gcp-us-central1",
  "Resources": "pod:1/10,volume:3/10,fip:5/10,service:-7/10,secret:1/11",
  "ServiceClusterIPRange": "10.96.0.0/12",
  "TenantID": "ab43de60c97a4120999249c00ff1eef3"
}
```
