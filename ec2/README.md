# EC2 Configuration

## Overview

The CV website is hosted on an Amazon EC2 instance running Amazon Linux 2023 with Apache HTTP Server.

## Instance Details

- **Instance name:** `cv-ec2-23l-3058`
- **Instance ID:** `i-05206484dfb765919`
- **Region:** `ap-southeast-2` (Asia Pacific — Sydney)
- **Instance type:** `t3.micro`
- **Operating system:** Amazon Linux 2023
- **Web server:** Apache HTTP Server (`httpd`)
- **Public IPv4 address:** `16.178.150.158`
- **IAM role:** `cv-ec2-role-23l-3058`

## Security Group

- **Security group name:** `cv-ec2-sg-23l-3058`
- **Inbound HTTP:** TCP port 80 from `0.0.0.0/0`
- **SSH:** No permanent public SSH access should be left open unless required for administration.
- **Outbound traffic:** Allowed as configured for the assignment.

## Website Synchronization

The instance uses its IAM role to retrieve website files from the private S3 bucket `cv-23l-3058`, under the `site/` prefix.

The synchronization script downloads the website files to `/var/www/html/` and replaces `{{HOSTED_ON}}` with the EC2 instance identifier. A systemd timer runs the synchronization periodically.

The synchronization log is stored at:

`/var/log/cv-sync.log`

## Verification

Verify the instance state, attached IAM role, security group rules, website response, and synchronization log in AWS before submission.
