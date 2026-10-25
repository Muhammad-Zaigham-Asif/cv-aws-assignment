# IAM Configuration

## Overview

This assignment uses AWS IAM roles to grant EC2 and Lambda temporary, service-managed permissions to access the private S3 bucket. No long-term AWS access keys are stored in application code or on the EC2 instance.

## EC2 Role

- **Role name:** `cv-ec2-role-23l-3058`
- **Trusted service:** EC2
- **Inline policy:** `cv-ec2-s3-read`

Permissions:
- `s3:GetObject` on `arn:aws:s3:::cv-23l-3058/site/*`
- `s3:ListBucket` on `arn:aws:s3:::cv-23l-3058`, restricted to the `site/` prefix.

## Lambda Role

- **Role name:** `cv-lambda-role-23l-3058`
- **Trusted service:** Lambda
- **Managed policy:** `AWSLambdaBasicExecutionRole`
- **Inline policy:** `cv-lambda-s3-read`

Permission:
- `s3:GetObject` on `arn:aws:s3:::cv-23l-3058/site/*`

## Security Notes

- The S3 bucket remains private.
- S3 Block Public Access remains enabled.
- Applications access S3 using IAM roles rather than embedded credentials.
- Permissions are restricted to the required objects and services.

## Verification

Confirm these role names, trust relationships, policies, and resource ARNs in the AWS console before submitting the assignment.
