---
layout: post
title: AWS CodeBuild Optimization using custom images
tags: CodeBuild AWS Optimization docker
---

Existing codebuild using  AWS provided docker/ruby templates for build/validation is working good, however noticed that in last few days,the build time is crossing beyond 1 minute 15 seconds, which gets charged as 2 minutes by AWS codebuild, hence was looking for ways to reduce the build time to less than 1 minute. 

Here are the phases of the build and time it spent in each phase ( 62 seconds for Install):

![Before](/assets/screenshots/codebuild_phases_before-20181216.png)

As noticed, most part of the build time is spent in Install phase, which is installing required gems for Jekyll build to work,Hence reducing this install phase should reduce overall build time.

AWS codebuild also supports custom docker images, Hence taking  AWS Codebuild docker image and customizing it with required gems,should reduce the runtime, Here are the summary of steps followed.

1. Obtain official AWS codebuild images from [here](https://github.com/aws/aws-codebuild-docker-images)
2. Customize ruby  dockerfile to install gems  after bundler [update](https://github.com/aws/aws-codebuild-docker-images/blob/master/ubuntu/ruby/2.5.3/Dockerfile)
3. Build docker image from dockerfile
4. Obtain the source code and run docker image locally on the host using codebuild local agent (https://docs.aws.amazon.com/codebuild/latest/userguide/use-codebuild-agent.html)
5. If everything works as expected, push the docker image to ECR repository
6. Change codebuild environment to use the custom docker image by referencing ECR/docker image by its tag
7. Validate codebuild works

Here are the phases of the build after change and time it spent in each phase (2 seconds for Install):

![After](/assets/screenshots/codebuild_phases_after-20181216.png)

From architectural perspective, this change improves  runtime significantly,gain further control over the  build process and improve runtime in future optimizations but also adds additional layer of maintenance by introduction of docker image and its ECR repository, but from operational standpoint, this change can be modified/removed with no code/application change by using codebuild environment to use standard images when ever required. Additional ECR repository adds fixed cost for stored docker images and data transfer in/out is not charged since all of this is within the same region of AWS, fixed cost to maintain ECR will breakeven only when more than 20 build minutes are used per month.

