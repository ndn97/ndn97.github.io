---
layout: post
title: S3 website public access prevention
tags: S3 Cloudfront AWS
---

S3 public websites have limitation to access only using http and not https, this can be addressed using cloudfront(http/https) as 
frontend of website and S3 public website as backend(http). However both frontend and backend can be accessed separately and can be considered
as duplicates.

To ensure access to S3 public website only from cloudfront, cloudfront can pass  origin header as Referer which S3 would whitelist using bucket policy 
as given below

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadForGetBucketObjects",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::swbucketname/*",
            "Condition": {
                "StringLike": {
                    "aws:Referer": "secretfors3"
                }
            }
        }
    ]
}
```
this effectively restricts all public access  limited to cloudfront,which caches frequent data and improves speed.
