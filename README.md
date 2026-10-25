# Host Your CV on AWS

## Overview

This project hosts a personal CV using Amazon S3, Amazon EC2, AWS Lambda, and AWS IAM. The S3 bucket stores the website files privately, while EC2 and Lambda serve those files through their respective endpoints.

## Architecture

```text
                         Website visitors
                         /              \
                        v                v
                EC2 public IPv4     Lambda Function URL
                  Apache HTTP       Python Lambda handler
                        \                /
                         v              v
                       Private Amazon S3
                     Bucket: cv-23l-3058
                         Prefix: site/
```

## Request Flow

For EC2, the browser sends an HTTP request to the instance's public IPv4 address. Apache serves the local website files, which are synchronized from the private S3 bucket using the EC2 IAM role.

For Lambda, the browser sends a request to the public Function URL. The Python handler reads the requested file from S3 using the Lambda execution role and returns the appropriate content type.

## AWS Resources

| Resource | Name |
|---|---|
| S3 bucket | `cv-23l-3058` |
| EC2 instance | `cv-ec2-23l-3058` |
| EC2 IAM role | `cv-ec2-role-23l-3058` |
| EC2 security group | `cv-ec2-sg-23l-3058` |
| Lambda function | `cv-func-23l-3058` |
| Lambda IAM role | `cv-lambda-role-23l-3058` |
| AWS Region | `ap-southeast-2` (Sydney) |

## Security

- S3 Block Public Access should remain enabled.
- The bucket should not have a public-read policy or ACL.
- EC2 and Lambda use IAM roles instead of long-term access keys.
- The EC2 role reads only the required `site/` objects and lists the permitted prefix.
- The Lambda role reads the required S3 objects and writes logs to CloudWatch.

## S3 Direct Access

Opening the direct S3 object URL for `index.html` should return `AccessDenied` because the bucket is private and public access is blocked. The website is served through EC2 or Lambda instead.

## Synchronization Log

The EC2 synchronization log is located at `/var/log/cv-sync.log`. Before submission, include the output of the last 10 lines of this log, as required by the assignment.

## Assignment Questions

### 1. Why use IAM roles instead of access keys? What could go wrong if keys were placed in user data?

IAM roles provide temporary credentials managed by AWS and avoid storing long-term access keys in application code or on the instance. Keys placed in user data could be exposed through instance metadata, logs, configuration access, or accidental sharing, allowing unauthorized access until the keys are revoked.

### 2. What happens when the direct S3 URL for `index.html` is opened, and which setting causes this?

The request should return `AccessDenied` because the bucket is private. S3 Block Public Access prevents public access configurations from granting public access, and the bucket has no public-read policy or ACL.

### 3. Why can anyone call the Lambda Function URL, and why is that acceptable for the function but not for the bucket?

The Function URL uses authentication type `NONE`, so callers do not need AWS IAM authentication to invoke it. This is acceptable for a deliberately public CV website endpoint if the handler exposes only intended website content. The S3 bucket should remain private so callers cannot directly list or retrieve arbitrary bucket objects.

### 4. How do costs compare when there is no traffic, and what happens if traffic increases 100 times?

An EC2 instance can incur compute charges while it is running even with no requests, whereas Lambda generally charges for requests and execution duration, with applicable free allowances and other charges. At 100 times the traffic, Lambda execution charges may rise with usage, while EC2 costs depend on instance capacity and whether scaling or additional infrastructure is required. Actual costs depend on region, workload, free-tier eligibility, and other services used.

### 5. If the EC2 instance is deleted, what is lost and what remains?

The EC2-hosted website endpoint stops working, and instance-local files, configuration, and logs may be lost unless separately preserved. The S3 bucket and its stored website files remain, as do the Lambda function and its endpoint, IAM roles, and other separately managed resources unless they are also deleted.

## Evidence

See `evidence.txt` for the AWS verification output. The evidence should be collected from the actual resources before submission.

## EC2
http://16.178.150.158/
<img width="1912" height="940" alt="image" src="https://github.com/user-attachments/assets/214d17a7-3033-4854-9f47-b63c00ae10e5" />

## Lambda function
https://a4edahpervknw5t34nf6nfpqdu0dcmuc.lambda-url.ap-southeast-2.on.aws/
<img width="1892" height="960" alt="image" src="https://github.com/user-attachments/assets/4fed159f-a1db-448f-b127-82c77c4aba9b" />
