---
layout: post
title: "Setting Up SSL on Github Pages Even If They Use a Custom Domain"
date: 2017-09-03
categories:
- blog
---

Since all browsers are trying to force [HTTPS](https://en.wikipedia.org/wiki/HTTPS){:target="_blank"}, it is highly recommended to setup [SSL](https://en.wikipedia.org/wiki/Transport_Layer_Security){:target="_blank"} on your domain name.
However for websites hosted on [Github pages](https://pages.github.com/){:target="_blank"} it was not optional because the SSL provided by Github was specific for `*.github.io` domains.  

Thanks to [CloudFlare](https://en.wikipedia.org/wiki/Cloudflare){:target="_blank"}, this became possible using the following steps:

1. Go to [cloudFlare.com](https://www.cloudflare.com/){:target="_blank"} and create a free account
2. Make sure that your `gh-pages` branch contains a CNAME file and the domain is added correctly
3. Add your website to CloudFlare, you will be provided new nameservers to use and they will contain all the old A records, which will make the transition from Github to Cloudflare easy with no downtime at all
4. Open CloudFlare Settings for your domain and Change the SSL to 'Full'  

<img src="../../images/2017_09_03_01.JPG" style="max-width:640px;">
5. Open Page Rules for your domain and add a new page rule, similar to the below image  

<img src="../../images/2017_09_03_02.JPG" style="max-width:640px;">

Please note that these changes may take up to 24 hours to propagate correctly. Until Github provides a native solution, this is the best way that I know of; If you have a better solution, please add it to the comments section.
