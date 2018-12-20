---
layout: post
title: S3 Failover to Github
tags: S3 Route53 Github AWS
---

AWS S3 storage and S3 websites are highest available services in terms of uptime, as they are backed by AWS SLA of 99.99% availability in a given year. AWS [health status](https://status.aws.amazon.com/) also leverages S3, however there are chances still S3 can go down, here is [report](https://www.theregister.co.uk/2017/03/01/aws_s3_outage/), where AWS S3 was down for few hours and AWS also had no access to  update the health status.

With this context, is there a way to automatically failover when highly reliably AWS S3 fails and still run your services?
since this site relies on github as a repo/editor, few months ago, github also started to support https on [custom domain names](https://blog.github.com/2018-05-01-github-pages-custom-domains-https/) , Hence github pages is right alternative for AWS S3 for static sites. So the change required is to add github pages as failover whenever underlying S3  fails or configuration/deployment failure to S3.

github pages are automatically available at username.github.io , need to update the Route 53 DNS record from existing simple/Alias record to Failover(Primary)/CNAME. github does not supports cname at apex domain(example.com), Hence need to resolve this using Alias record which redirects example.com to www.example.com ( setting redirect at S3 bucket).

Route 53 health check is configured to check the status of website using index.html page over port 80 at S3 public endpoint,this would be free of charge as it is using AWS endpoint, default health check would check every 30 seconds from more than 10 s3 regions, this healthchecks would generate around 1,000,000 get request against the S3 bucket, made changes to reduce this to minimum required 3 regions to reduce this flooding of the requests.

Created second record as Failover(Secondary)/CNAME record with target as username.github.io without any healthcheck,to simulate the failover,changed S3 index.html object to be unavailable, here is how DNS health check finds it and DNS failover works when this occurs.

* DNS healthcheck Notification over email
![DNS healthcheck Notification](/assets/screenshots/20181220-dnshealthcheck.png)

* DNS response on failure (points to username.github.io)
![DNS response on failure](/assets/screenshots/20181220-dnsresponse.png)

On failover to secondary(github pages), website worked with fully functionalty including support for https for both apex and subdomain as required. Main advantage with this change is  automatic standby/failover and cost optimizations if the primary website using AWS becomes costly to manage (CDN/S3 charges), can easily make Secondary as Primary by just modifying an object in S3/Route 53.

