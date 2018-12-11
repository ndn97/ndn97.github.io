---
layout: post
title: Website SEO optimization
---

For self-managed websites, Optimization for SEO need to be considered for improving search engine rankings, I validated the current SEO rankings from multiple sites for this site, main fixes recommended by lots of SEO checkers are:
  
  1. HTML compressions for faster page loads
  2. Enable sitemap/robots.txt
  3. CSS Minification 
  4. Social Media Check
  5. Key work Usage Test
  
  
Quantified score for this website before optimiation was 71/100
![Before SEO](/assets/screenshots/before%20SEO.png)

After performing HTML compression and enabling sitemap and robots.txt, the score improved to 84/100
![After SEO](/assets/screenshots/after%20SEO.png)


This is good improvement, will work towards improving the score to 90/100.

Update 1: Cloudfront was not showing custom 404 error page, this is due to directly using S3 bucket as Origin for cloudfront, when changed this  to use S3 public endpoint,it fixed the issue

Update 2: After implementing url cannonical implementation as discussed in [here](https://nageshdn.com/2018/12/10/https-and-URL-canonical-support-from-S3.html), the score is as 92/100
![After SEO 2](/assets/screenshots/update2_seo.png)
