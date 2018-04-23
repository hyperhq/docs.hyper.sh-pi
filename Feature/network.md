# Network

![](https://trello-attachments.s3.amazonaws.com/5700ea0da7030dcf7485ed70/5a97dd05ecefd109f1b7e367/d515b403538d8e9f040ed1b20a9eb486/4.png)

- Every account is assigned a default Layer-2 virtual private network in each region.
- Networks are isolated from each other and the Internet.
- The [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) of the network is set to `172.16.0.0/16`. Note: We reserve a few addresses of your private network for the platform's own usage.
- All your pods will be automatically placed in your own network. 
- Every pod receives a unique private IP address (aka _Pod IP_) upon startup, which is specific to the network the pod resides in. There is currently no way to control the allocation of _Pod IP_.
- _Pod IP_ address is static during the pod lifespan and is only returned when the pod is terminated.
- _Pod IP_ is shared by all containers in a pod, therefore you must ensure no port conflicts among containers within a pod.
- Pods in the same network are reachable to each other (using _Pod IP_), but isolated from other networks (hence customers).
- Pods come with the access to the Internet by default, but they are not accessible from the Internet. This makes your application more secure as you can't accidently expose an important part of your infrastructure to the Internet.
- A network has a virtual router, whose IP address may vary over time. Pods use the virtual router for outgoing traffic.
- _Service_ serves as load balancer in front of a fleet of pods. Each service is created with a private IP (_Service IP_) and an internal DNS name (_Service DNS Name_), which are accessible by other pods within the same network.
- When associated with _Floating IP_, the _service_ is exposed on the Internet. Traffic sent to the _Floating IP_ address will be balanced among the backend pod fleet.
- One _service_ can bind only one _Floating IP_, but multiple _services_ can be associated with a single _Floating IP_, as long as no port conflicts among different _services_. This allows you to use different ports for different sub-systems, but with a single public IP address.
