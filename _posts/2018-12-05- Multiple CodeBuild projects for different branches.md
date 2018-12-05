---
layout: post
title: Separating build projects for different branches
---

By Integrating GitHub repository with AWS CodeBuild, it monitors every change in the repository and builds and deploys the website for every change,this is not desirable, Hence need to separate the Build and Deployment in the repository to different CodeBuild Projects. I have configure 2 build projects in codebuild one for master branch and one for draft branch, with this github will have 2 webhooks in the repository, which can be customized for which events it should trigger.

For posts which are in the draft branch, which goes through multiple revisions while drafting should not trigger build, to meet this, marked only Pull Requests as option in webhooks of the repository, with this it only builds whenever I wish to merge the draft branch with master branch using pull request and the output of the build will be available within the pullrequest itself.

For posts which need to be published, whenever the pull request from draft branch is merged with master branch, it causes a Push event in master branch which is monitored by the webhook  and runs build and deploy.This changes should optimize the usage build minutes further as discussed in the post [here](https://nageshdn.com/2018/12/05/Migration-from-Travis-CI-to-AWS-CodeBuild.html)

