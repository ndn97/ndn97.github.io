---
layout: post
title: AWS CodeBuild Optimization using custom images
tags: CodeBuild AWS Optimization docker
---

Existing codebuild using the AWS provided ruby templates for build/validation is working good, however noticed that in last few days,the build time is crossing beyond 1 minute 15 seconds, which gets charged as 2 minutes by AWS codebuild, hence was looking for way to reduce the build time to less than 1 minute. 

Here are the phases of the build and time it was taking:

![codebuild_phases_before_20181216](/assests/screenshots/codebuild_phases_before_20181216.png)

As noticied, most part of the build time is spent in Install phase, which is installing required gems for Jekyll build to work,Hence reducing this install phase should reduce the overall build time.

AWS codebuild also supports custom docker images, Hence taking the AWS Codebuild docker image and customizing it with required gems,shoudl reduce the runtime, Here are the highlevel steps to do the same.

1. Obtain official AWS codebuild images from [here](https://github.com/aws/aws-codebuild-docker-images)
2. Customize ruby  dockerfile to install gems  after bundler [update](https://github.com/aws/aws-codebuild-docker-images/blob/master/ubuntu/ruby/2.5.3/Dockerfile)
3. Build docker image from dockerfile
4. Obtain the source code and run docker image locally on the host ()
5.If everything works as expected, push the docker image to ecr repository
6.Change the codebuild environment to use the custom docker image by referencing your ECR/docker image by its tag
7.Validate codebuild works

Here are the


