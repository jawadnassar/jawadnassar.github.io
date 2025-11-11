---
layout: post
title: "Blocking AI bots from scraping your site"
date: 2025-07-14
categories: Blog
---

I noticed that a large percentage of traffic to my site comes from AI bots. 

Even though many of them do not comply with `robots.txt` or `ai.txt`, I’ve included the following lists in an attempt to block some of them:

## robots.txt
```
---
layout: null
sitemap: false
---
User-agent: *
Sitemap: {{ "sitemap.xml" | absolute_url }}

User-agent: CCBot
Disallow: /

User-agent: ChatGPT-User
Disallow: /

User-agent: GPTBot
Disallow: /

User-agent: Google-Extended
Disallow: /

User-agent: Google-CloudVertexBot
Disallow: /

User-agent: Applebot-Extended 
Disallow: /

User-agent: anthropic-ai
Disallow: /

User-agent: ClaudeBot 
Disallow: /

User-agent: Omgilibot
Disallow: /

User-agent: Omgili
Disallow: /

User-agent: FacebookBot
Disallow: /

User-agent: Diffbot
Disallow: /

User-agent: DuckAssistBot
Disallow: /

User-agent: AI2Bot
Disallow: /

User-agent: Bytespider
Disallow: /

User-agent: Kangaroo Bot
Disallow: /

User-agent: PanguBot
Disallow: /

User-agent: ImagesiftBot 
Disallow: /

User-agent: PerplexityBot
Disallow: /

User-agent: cohere-ai
Disallow: /

User-agent: cohere-training-data-crawler
Disallow: /

User-agent: Meta-ExternalAgent
Disallow: /

User-agent: Meta-ExternalFetcher
Disallow: /

User-agent: Timpibot
Disallow: /

User-agent: Webzio-Extended
Disallow: /

User-agent: YouBot
Disallow: /

User-agent: Onespot-ScraperBot
Disallow: /
```

For an updated list, keep an eye on this [thread](https://neil-clarke.com/block-the-bots-that-feed-ai-models-by-scraping-your-website/){:target="_blank"}.

## ai.txt
```
# Spawning AI
# Prevent datasets from using the following file types

User-Agent: *
Disallow: *.txt
Disallow: *.pdf
Disallow: *.doc
Disallow: *.docx
Disallow: *.odt
Disallow: *.rtf
Disallow: *.tex
Disallow: *.wks
Disallow: *.wpd
Disallow: *.wps
Disallow: *.html
Disallow: *.bmp
Disallow: *.gif
Disallow: *.ico
Disallow: *.jpeg
Disallow: *.jpg
Disallow: *.png
Disallow: *.svg
Disallow: *.tif
Disallow: *.tiff
Disallow: *.webp
Disallow: *.aac
Disallow: *.aiff
Disallow: *.amr
Disallow: *.flac
Disallow: *.m4a
Disallow: *.mp3
Disallow: *.oga
Disallow: *.opus
Disallow: *.wav
Disallow: *.wma
Disallow: *.mp4
Disallow: *.webm
Disallow: *.ogg
Disallow: *.avi
Disallow: *.mov
Disallow: *.wmv
Disallow: *.flv
Disallow: *.mkv
Disallow: *.py
Disallow: *.js
Disallow: *.java
Disallow: *.c
Disallow: *.cpp
Disallow: *.cs
Disallow: *.h
Disallow: *.css
Disallow: *.php
Disallow: *.swift
Disallow: *.go
Disallow: *.rb
Disallow: *.pl
Disallow: *.sh
Disallow: *.sql
Disallow: /
Disallow: *
```

## Meta tag
Adding the below meta tag to HTML headers

```html
<meta name="robots" content="noai, noimageai, DisallowAITraining">
```

