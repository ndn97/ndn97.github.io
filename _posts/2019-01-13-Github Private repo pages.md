---
layout: post
title: Github Private repo for hosting pages
tags: Github Private Hosting
---

This website uses github as repository/editor for markdown and  AWS for distribution using S3/Cloudfront. Github repo has to be public/open if it is not subscribed to paid plans. The other alternative to have private repo is bitbucket or AWS codecommit.

Earlier this month github announced  unlimited free private repository with collobarators limited upto 3 users. Hence moved this repository to private, However this breaks github pages features, which works only if it is public or pro account. Since this site is using github pages only as failover (when AWS S3 is down). 

![Disabled Pages for private repo ](/assets/screenshots/github_private_pages.png)

Need to explore alternatives for backup/github pages for private repo.



