# Pricing & Billing

_Pi_ use a _pay-per-use_ billing model, similar to other cloud services, such like AWS EC2. You pay only for the resources you use by the second, no minimum fee. 

> To see your bill, go to your [billing history](https://console.hyper.sh/billing/history). Your bill contains links to usage reports that provide details about your bill. 

#### Compute
	
- **Image**:
	- Image is a self-contained package that includes application, configuration and data. _Pi_ will pull your images from 3rd-party registry and launch them. We do not offer our own registry at the moment.
	- Currently image is **FREE**.

- **Pod**:
	- Pod is charged by the computational resources it consumes (CPU, RAM) and the duration it runs (by the second). 	
	- We provide a set of predefined resource configurations for you to choose when launching pods. Different configurations come with different resource specifications, thus different price.  
	- Billing begins when a pod is started, ends when the pod terminates (through API call), or exits (success or failure).
	- Everytime a pod is started, a default duration of 60 seconds is applied. If a pod runs for 40s, it will be billed for 60s; if it runs for 100s, it is billed for 100s.

|Size|CPU|Mem|Per Second|
|:-:|:-:|:-:|:-:|
|S1 |1|64MB |$0.00000222|
|S2 |1|128MB|$0.00000354|
|S3 |1|256MB|$0.00000564|
|S4 |1|512MB|$0.000009  |  
|M1 |1|  1GB|$0.0000144 |
|M2 |1|  2GB|$0.0000288 |
|M3 |1|  4GB|$0.0000576 |
|L1 |2|  8GB|$0.0001152 |
|L2 |4| 16GB|$0.0002304 |

#### Automatic Sustained Use Discounts
Similar with [Google Compute Engine](https://cloud.google.com/compute/pricing), your Pods will qualify for a sustained use discount. The discounts are applied automatically and will be calculated and added to your bill. There is no action needed on your part to enable sustained use discounts.

|Usage Duration (second)|% at which incremental is charged|Example (M1 size)|
|:-:|:-:|:-:|
|0-600    | 100% of base rate | $0.0000144/second |
|600-3600 |  67% of base rate | $0.0000096/second |
|3600+    |  33% of base rate | $0.0000048/second |

> **Example**:
> For a _M1_ pod to run for an entire month, the pod will cost:
> - First 600 seconds: 600*$0.0000144 = $0.00864
> - The next 3000 seconds: 3000*$0.0000096 = $0.0288
> - The rest of the month: 3600*(24\*30-1)*$0.0000048 = $12.42432
>
> - Total: $0.00864 + $0.0288 + $12.42432 = $12.46176
 
#### Storage

- **Rootfs**:
	- A pod may have one or more containers.
	- Every container come with a default 10GB rootfs, e.g. if a pod have 3 containers, the pod has three rootfs, 30GB in total.
	- Rootfs exits along with the lifecycle of the pod. Billing begins when the pod is created, ends when the pod terminates or exists. 
	- A default duration of 60 seconds is applied.

- **Volume**:
	- The volume size ranges from 10 to 50 GB, and the default size is 10GB.
	- Volume is independent from the pod's lifecycle. Thus, the billing begins when a volume is created, ends upon removal.
	- A default duration of 60 seconds is applied.

 - **Secret**: Free

|Type|Per Second|Monthly|
|---|---|---|
|Rootfs|$0.0000000386/GB|$0.1/GB|
|Volume|$0.0000000386/GB|$0.1/GB|

#### Network
- **Traffic**:
	- Currently all network traffic are **Free**.
- **Service**: 
	- $0.03/hour
	- Billing begins when a new service is created, ends when it is deleted. Partial hour will be counted as an hour.
	- A default duration of 1 hour is applied.
- **Floating IP**: 
	- If you allocate a floating IP address but do not use it, you will be charged with an hourly rate. If you use it (with _Service_), you will not be charged for it.

|Type|Hourly|
|---|---|
|Floating IP address (assigned but unused)|$0.01/IP|
|Floating IP address (assigned and in use)|Free|

# FAQ

#### What payment types do you accept?
Currently, _Pi_ accepts credit or debit cards, including Visa, MasterCard, American Express, JCB, Discover, and Diners Club.

#### Does _Pi_ keep my credit card information?
No, all payments are processed through Stripe, a trusted third party payment processing service. _Pi_ does not retain any credit card information.

#### Why was my card declined?

Our third party payment processing service employs specific checks against card payments. Some of the reasons your card was declined could be:

1. **The zip code you supplied failed validation.**: You can leave the Zip Code as blank and try again if your card not located in US.
2. **Your card's security code is incorrect.**: Currently we require CVC verification.
3. **Your card does not support this type of purchase.**: Please get in touch with your bank to confirm the reason declining the payments.
4. **Your card was declined.**: If you make sure all fields was filled in correctly, please get in touch with your bank to confirm the reason declining the payments.

#### When will my credit card be charged?
We charge your credit card on the 1st day of each month.

#### How about the billing tax?
Except as otherwise noted, our prices exclude applicable sales tax and VAT. Currently, we charge US sales tax for credit cards issued in the US states of NY and Texas and VAT tax for non-business users based in EU. The tax details will be available in the monthly bill.

#### What happens after outstanding balance?

- First Payment Attempt: Credit card payment charged on 1st of the month. You will receive email notification of failed credit card charge and to update credit card information in web console.
- Second Payment Attempt: Submitted 3 days after first attempt, if the first charge fails, during this period you will be unable to create / update resources in your account.
- Third Payment Attempt: Submitted 5 days after second attempt, if the second charge attempt fails. If third attempt fails, we will stop all pods in your account.
- If the balance is due over 30 days, we will deactivate your account (unable to login to web console, removal of all credentials), and permanently purge all resources in your account.
- You can login the web console to update your credit card information, which will trigger new payment request, whenever a payment request succeeds, we will immediately resume your account.

If you have any questions about an outstanding balance, how to add a payment option, or about billing in general, please feel free to contact [s Support](mailto:support@hyper.sh).

#### How do I remove my credit card?

You can go to https://console.hyper.sh/billing/credit to add/update/delete your credit card.
- First please make sure that there is no payment due and that there are no resources left in your account.
- If there is payment due, you will need to update the credit card to first fullfill the payment.
- Once the credit card information is removed, your account will be limited. You can add a credit card to re-enable your account.
