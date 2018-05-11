# Quota & Limits

_Pi_ has certain hard and soft (`quota`) limits in using its service. Hard limits are automatically enforced by the service. Soft limits are consumable resources that you may request to increase:

## Hard Limits
- Max image size: 10 GB
- Max containers per pod: 32
- Max flex volumes per pod: 4
- Max pod json size: 128kb
- Max secret json size: 4kb
- Rootfs size: 10 GB
- Volume size: 1 GB - 1024 GB

## Quota
- Pod: 20 per region
- Volume: 40 volumes per region
- Service: 5 per region
- Floating IP: 5 per region
- Secrets: 8 per region
- API Credential: 3 per region

### Request a quota increase

Open the [Overview](https://console.hyper.sh/overview/) page, click `Request Quota Increase`:

![](https://trello-attachments.s3.amazonaws.com/5700ea0da7030dcf7485ed70/57ac415d5c5774e392d184a5/54e79f48f900bc73590d58964c80cd92/quota.png)

In the dialog popup, fill in your request and use case details. It usually takes 1-2 business days for us to review the request and get back to you.
