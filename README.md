# ec2-idle-check - stop instance when there are no active users

## Purpose

If you use an EBS-backed EC2 instance for development, you can reduce your EC2 costs by starting it when you need it, and stopping when you're done. But of course, we all forget that last step sometimes.

## Dependencies

* ruby
* curl
* ec2-api-tools

## Usage

Run this script at startup (from `/etc/rc.local` or similar) and it will daemonize and check every 60 seconds whether there are any users logged in and active. If there are no users logged in, or none of them have been active in the past 15 minutes, the EC2 instance is stopped.

If `/etc/profile.d/aws-apitools-common.sh` is present on your system, you should source it first before running `ec2-idle-check`:
```bash
. /etc/profile.d/aws-apitools-common.sh
```

EBS-backed instances can typically be safely stopped and started without data loss, but remember that any instance-based stores will become inaccessible if your instance stops. Instance-based stores should only be used for temporary storage.

This dev setup works best if you have a dynamic DNS service set to update your instance's DNS entry on boot. If you are using Route 53, install the `aws-sdk-route53` gem and run the `ec2-update-dns <hostname> <route53-hosted-zone-id>` script at startup.

## License

Distributed under the MIT license.
