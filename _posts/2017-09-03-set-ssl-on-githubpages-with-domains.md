---
layout: post
title: "HTTPS on Github Pages with a Custom Domain"
date: 2017-09-03
categories: Lab
---

> Update: [Custom domains on GitHub Pages gain support for HTTPS](https://blog.github.com/2018-05-01-github-pages-custom-domains-https/){:target="_blank"}

Since all browsers are trying to force [HTTPS](https://en.wikipedia.org/wiki/HTTPS){:target="_blank"}; it is highly recommended to setup it up on your domain name; however for websites with custom domains hosted on [Github pages](https://pages.github.com/){:target="_blank"} it wasn't supported because the certificates provided by Github is specific for `*.github.io` domains.  

Thanks to [CloudFlare](https://en.wikipedia.org/wiki/Cloudflare){:target="_blank"}, this became possible using the following steps:

1. Go to [cloudflare.com](https://www.cloudflare.com/){:target="_blank"} and create a free account
2. Make sure that your `gh-pages` branch contains a CNAME file and the domain is added correctly
3. Add your website to CloudFlare, you will be provided with new nameservers to use and they will contain all the old A records, which will make the transition from Github to Cloudflare easy with no downtime at all
4. Open CloudFlare settings for your domain and Change the SSL to 'Full'  

<img src="https://jawad.ca/images/2017_09_03_01.JPG" >
5. Open page rules for your domain and add a new page rule 

<img src="https://jawad.ca/images/2017_09_03_02.JPG">

Please note that these changes may take up to 24 hours to propagate correctly. Until Github provides a native solution, this is the quickest way to accomplish this.
