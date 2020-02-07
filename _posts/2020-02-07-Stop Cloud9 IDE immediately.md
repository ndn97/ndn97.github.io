---
layout: post
title: Stop Cloud9 IDE immediately to save costs
tags: Cloud9 EC2 AWS
---

AWS cloud9 IDE simplifed access and development in Cloud by using cloud native IDE and associated terminal for CLI access.
To reduce wastage of unused EC2 instances which are the backend of Cloud9, Cloud9 IDE detects presence of user based on activity 
and shutdowns EC2 instances using predetermined interval.

The minimum time to configure inactivity is 30 minutes, which means when user is not using cloud9  upto 30 mins, Cloud9 shutsdown
EC2 instance.

To enable immediate shutdown of cloud9 environment on determinstic use cases, user can accomplish this by following process.

1.write lambda function which stops the requested EC2 instance ID. simple code for this is..

```python
import boto3
region = 'us-east-1'
ec2 = boto3.client('ec2', region_name=region)
def stopEC2(event, context):
    print("Received Instance ID:"+event['queryStringParameters']['instance_id'])
    if(
            event['httpMethod'] == 'GET' and
            event['path'] == "/someconfidentialpath/ec2stop" and
            event['queryStringParameters']['instance_id'] != ''
            ):
        instance_id=event['queryStringParameters']['instance_id']
        ec2.stop_instances(InstanceIds=[instance_id])
        print('stopped your instances: ' + instance_id)
```

2. Deploy the above lambda function with IAM role which has permission to Stop EC2 instances.

3. Configure AWS API Gateway which proxies the request to above given lambda function.

4. Configure a http bookmark for API Gateway with querystring parameters as instance ID.
 e.g: https://api.nageshdn.com/someconfidentialpath/ec2stop?instance_id=i-012345678923

Whenever user is leaving the desk, user just need to click the above http bookmark, which will trigger lambda function to shutdown
EC2 instances and saves costs upto 30 mins every time.


