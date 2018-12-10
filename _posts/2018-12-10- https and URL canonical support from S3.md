---
layout: post
title: https and URL canonical support from S3
tags: s3 cloudfront website ACM
---

The main disadvantage of hosting static websites from S3 is, there is no native https/SSL enabled for S3 website endpoints,it supports only http endpoint, to get around this, https/SSL termination can be enabled at cloudfront distribution for S3 website using ACM feature of AWS, so websites accessed using cloudfront ( Route 53 and cloudfront alias), will support http/https traffic. More details on this on enabling https for S3 using cloudfront can be found  [here](https://aws.amazon.com/premiumsupport/knowledge-center/cloudfront-https-requests-s3/)

For redirecting the www traffic to root-domain, most recommendations are to have additional bucket as www.example.com and set 301 redirect to example.com, However this continues to fail the URL cannonical test for SEOs, which is https/http and www/non-www pages  should all work.
  
    


| | non-www | www  |   |
| --- | --- | --- | --- |
| http | http://nageshdn.com (Pass)| http://www.nageshdn.com (Pass)| |
| https | https://nageshdn.com (Pass) | https://www.nageshdn.com (Fail)| |
|    |   |   |  |

As workaround, have to overcome https termination for  https://www.nageshdn.com at layer before S3 using cloudfront, hence setup additional cloudfront distribution which will perform https offloads to only subdomain www and the backend S3 bucket ( which is empty) will just redirect www.nageshdn.com  to [nageshdn.com](http://nageshdn.com), to support this additional cloudfront distribution, have to modify AWS certificate manager(ACM) to be applicable to wildcard subdomains (*.nageshdn.com )

Per [AWS well architected framework](https://aws.amazon.com/architecture/well-architected/), this changes should

    - Security : Increase security of the architecure, as www traffic will also support https
    - Performance: for www bound traffic,there is additional step of redirect , which is cached at cloudfront and uses alias for faster   performance
    - Operational Excellence: all www/non-www and http/https is supported, additional cloudfront distribution does not need any maintenance.
    - Cost: there is no additional cost incurred, since existing traffic is divided between 2 distributions and converged in the backend
    - Reliability: there is no additional failure aspects introduced by this change

After setting this up, I see URL cannonical test works for this website.
