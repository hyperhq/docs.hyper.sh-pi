# Region and Availability Zone

Same with other platforms like [_Heroku_](https://heroku.com), _Pi_ does not run in our own datacenter. Instead, it runs on [Google Cloud](https://cloud.google.com/). Therefore, the concept of region and availability zone in _Pi_ is the same as GCP.

- **Regions** are separate geographic physical data centers, completely indepedent from one another. Resource quota are region-specific.

- **Availability Zones** are isolated physical locations within a region, interconnected through low-latency links.

The current regions and availablity zones are:

<table class="table table-bordered table-striped table-condensed">
<tr>
<td>Cloud</td><td>Region</td><td>Location</td><td>Zones</td>
</tr>
<tr>
<td>Google Cloud</td><td>gcp-us-central1</td><td>Iowa</td><td>gcp-us-central1-a,gcp-us-central1-c</td>
</tr>
</table>

### Resource Location
In Pi, resources are either global, specific to a region, an availability zone. However, when you view your resources in the [web console](https://console.hyper.sh), you'll only see the resources tied to the region you've specified.

<table class="table table-bordered table-striped table-condensed">
<tr>
<td>Resource</td><td>Type</td><td>Description</td>
</tr>
<tr>
<tr>
<td>API Credential</td><td>Global</td><td>You can use the same API credential in all regions.</td>
</tr>
<tr>
<td>Pod</td><td>Availability Zone</td><td>An instance is tied to the Availability Zones in which you launched it. However, its ID is tied to the region.</td>
</tr>
<tr>
<td>Volume</td><td>Availability Zone</td><td>A volume is tied to its Availability Zone and can be attached only to pods in the same Availability Zone.</td>
</tr>
<tr>
<td>Private Network</td><td>Region</td><td>A network is tied to a region.</td>
</tr>
<tr>
<td>Service</td><td>Region</td><td>A service is tied to a region and can be associated with pods from different availability zones in the same region.
</td>
</tr>
<tr>
<td>Floating IP</td><td>Region</td><td>A flooating IP is tied to a region and can be associated with services in the same region.
</td>
</tr>
<tr>
<td>Secret</td><td>Region</td><td>A secret is tied to a region.
</td>
</tr>
</table>

### Default Availability Zone
_Pi_ allows you to place resources in different locations. When you launch a pod or create a volume, you can choose an availability zone. Otherwise the resource will be placed in the default zone of your account. The default zone is selected upon your registration. As we allocate the default zone randomly, different accounts may have different default zones.

You can see your default zone in the [web console](https://console.hyper.sh/pi/info).

### API Endpoints

When you work with _Pi_ APIs, you must specify its regional endpoint:

<table class="table table-bordered table-striped table-condensed">
<tr>
<td>Cloud</td><td>Region</td><td>Location</td><td>API Endpoint</td>
</tr>
<tr>
<td>Google Cloud</td><td>gcp-us-central1</td><td>Iowa</td><td>tcp://gcp-us-central1.hyper.sh:443/api/v1.9</td>
</tr>
</table>

For more information about the API access, see our [API reference](../Reference/API/v1.9/index.md).
