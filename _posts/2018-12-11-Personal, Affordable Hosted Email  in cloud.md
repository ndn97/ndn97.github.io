---
layout: post
title: Personal,Affordable Hosted Email  in cloud
tags: Email SES AWS 
---

Personal/Business hosted email services from  Google Gsuite, Office 365, Amazon workmail etc is default choice for most pro users, the main advantage of these managed email services are large mailbox storage(upto 50G), archive/retention capability and other integrations with third party services. typical costs of these hosted email services are based on monthly rental and other addon features, which gets charged irrespective of actual usage.

What if there is  hosted email service, dedicated,gets charged based on actual transaction/usage charges? came across this project called [ses-lambda-forwarder](https://github.com/arithmetric/aws-lambda-ses-forwarder), which is a lambda function and uses inbound/outbound capabilities of AWS Simple Email Service (SES) to run a serverless email forwarding service, details of how to configure it are available with the project itself. 

AWS SES is good fit for mostly API based usage (transactional emails,marketing which are triggerd automatically)  and does not come with any mail client to check/send emails. AWS SES allows rulesets for processing mails using lambda, and can be used to store/forward them as required using lambda function. one can get the benefit of unlimited storage (gmail/office provides between 30-50G storage per user) from S3,can be used for analytics,archival in glacier for any number of years,  gain  control over the underlying mail infrastructure i,e email/compute/storage.

To integrate AWS SES to mail client of choice such as outlook/gmail, setting one time sync with smtp credentials is all required to compose/send/receive mails. Since AWS SES, can only send mails on behalf of  verified domain name, mails recieved/forwarded to the mail client has unattended email-address such as no-reply/info as it is forward message, Any reply to such mail will still go to intended user (uses reply-to option).

Expected solution cost for 1 email user, to send/receive  1MB size email, 100 mails per day for 20 days a month, are as follows 
- Total mails: 2000 emails @ 2GB storage and associated transfer charges

- SES charges: upto 1000 send and 1000 received mails are free per month for lifetime, attachment charges is around $0.12/GB,transfer charges are free for 1st 1GB and charged $.09 thereafter, overall charges could be around $0.33
- Compute(2000 lambda invocations of 128 MB,20 sec time) : 5200 GB-sec, $0 ( within free tier limit for lifetime)
- Storage charges: $0.2 for 2GB + $0.1(metadata) ,$0.3

overall solution cost will be around $0.7/user at start, as the mails grow month on month, accumulated storage and its costs will increase proportionally. currently this is adopted as choice of email for this site, reach out to me to check this [here](mailto:ndn@nageshdn.com). 
