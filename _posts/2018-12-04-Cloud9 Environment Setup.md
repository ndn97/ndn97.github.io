---
layout: post
title: Cloud9 Environment Setup
---

AWS Cloud9 provides IDE in the Browser, which is very good usecase for users who are on the go or limited time developers.The main considerations for this setup is to have a pilot light environment, which can scale if the need requires with minimal costs.with average utilization of the ~0.5 hours/Daily for 20 days, 10 hours/Month

To meet this design considerations, 
  1. Environment is setup with Burstable Compute Instances - T2/T3 Instance Type
  2. Instance Storage EBS will be limited to 8GB and will use standard Magnetic Type Disks ( not expecting major IO bound activity)
   
T3 Instances are latest Intel Skylake based processor which has unlimited bursting capability, T3.Nano(2 VCPU,0.5GB Memory @ $0.0052/hr )  priced around same as T2 instances(1 vCPU,0.5GB@ $0.0058/hr), However T3 Instances are not available from Cloud9 Setup, Hence used T2 instances for currently.


Cloud9 by default uses SSD based EBS(gp2), which is charged irrespective of the instance usage, Hence need to reduce this cost further, Hence opted to go with Standard HDD based EBS volume, for 8GB, which is $0.045 GB-Month vs $0.1 GB-Month. To convert  gp2 EBSe to magenetic EBS, just stop Cloud9 env/EC2 instance, and change the EBS volume type and open the cloud9 IDE again to get this effective.

Expected charges for 10 hours/month is $0.6 (0.2 for compute + 0.4 for Storage), I  plan to use this Env as baseline and  further optimize for price/performance with below changes.

  1. Move from T2 to T3 instance when availalbe in cloud9
  2. Modify the T2/T3 instance to Unlimited Bursting to sustain peak usage if required
  3. Change the volume size/type if performance warrants
