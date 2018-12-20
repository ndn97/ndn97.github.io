---
layout: post
title: Failover,Optimizations using Route 53 and Github pages
tags: Route53 Failover AWS Optimization 
---

AWS S3 storage and websites using S3 are one of the highest available services, as they are backed by AWS SLA of 99.99% availability in a given year. AWS health status also leverages S3 , however there are chances still S3 can go down, here is [report](https://www.theregister.co.uk/2017/03/01/aws_s3_outage/) where AWS S3 was down for few hours and AWS also had no access to  update the health status.

With this context, is there a way to automatically failover when highly reliably  AWS S3 fails and still run your services?
since this site relies on github as a repo/editor, few months ago, github also started to support https on [custom domain names](https://blog.github.com/2018-05-01-github-pages-custom-domains-https/) , Hence github pages is right alternative for AWS S3 for static sites. So the change required is add github pages as failover whenever underlying S3  fails or configuration/deployment failure to S3.

github pages are automatically available at <username>.github.io , need to update the Route 53 DNS record from existing simple/Alias record to Failover(Primary recordset)/Cname record. github does not supports cname at apex domain(example.com), Hence need to resolve this to using Alias record which redirects example.com to www.example.com  setting redirect at S3 bucket.

Route 53 health check is configured to check the status of website using index.html over port 80 at S3 public endpoint,this would be free charge as it is using AWS endpoint, default health check would check every 30 seconds from more then 10 s3 regions, however this would generate around 1,000,000 get request against the S3 bucket, made changes to reduce this to minimum required 3 regions.

Created Second Recordset as Faiover(Secondary)/CNAME recordset with target as <username>.github.io without an healthcheck,to simulate the failover,made S3 object to unavailable, here is how DNS health check finds it and DNS failover.

* DNS healthcheck Notification
![DNS healthcheck Notification](/assets/screenshots/20181220-dnshealthcheck.png)

* DNS response on failure
![DNS response on failure](/assets/screenshots/20181220-dnsresponse.png)

On failover to Secondary, website worked with fully functionalty including support for https for both apex and subdomain as required.Main advantage with this change is having automatic backup/failover and future cost optimizations if the primary using AWS becomes costly to manage (CDN/S3 charges), can easily make Secondary as Primary by just modifying an object in S3.

