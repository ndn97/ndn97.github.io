---
layout: post
title: Migration from Travis CI to AWS CodeBuild
---

Since this is a static website, conversion of markdown to html was performed by Jekyll engine whenever any commit to github repository is performed. Travis CI was very good to integrate with  github for all build/deployment of this website.The website has dependency on mainly 3 services viz github for editing markdown,Travis CI for build/deployment, AWS for hosting/caching the website.

To further optimize the dependencies, decided to remove Travis CI from build/deployment since this is no cost service, at times, it takes long time to get build started. AWS codebuild service seems right fit as this is managed build service from AWS, which natively integrates with other services such as S3 and Cloudfront, The other consideration in favor of AWS codebuild, is that it has  lifetime free usage of 100 build minutes per month on a small build instance (2 vCPU, 3GB Memory), after this free usage, the charges are $0.005/minute, on average each build should not last for more than 2 minutes, hence upto 50 builds per month is free. To remove Travis CI depencency, i just renamed the .travis.yml into .travis-orig.yml and added buildspec.yml as codebuild requirements.

The other major issue with static website using cloudfront is, website will take upto 24 hours for cache invalidation and new posts will appear only after this, which sometimes results in having stale posts. Cache-Invalidation on cloudfront purges the cloudfront cache data using invalidatiion request, Each invaldation request has associated costs at $0.005 per path,to reduce this invalidation costs,just added wildcard invalidation for all html files, since cloudfront still considers wildcard invalidation as just 1 request instead of number of objects which are invalidated.

with wildcard invalidation, this will purge all changed/unchanged files from cloudfront, any new request to cloudfront need to be fetched again from origin to cloudfront, this transfer of data is charged as "Regional Data transfer out to origin" which is $0.020 per GB-month,  will further lookforward to optimize this charges/performance.. 
