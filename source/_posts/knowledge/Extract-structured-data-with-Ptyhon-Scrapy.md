---
title: Extract structured data with Ptyhon Scrapy
date: 2019-03-10 16:54:37
tags: python
category: knowledge
---


scrapy commands
- scrapy bench: run a quick benchmarking test for how Scrapy runs on your hardware (memory and disk availability)
  - setup a local HTTP server and crawls it at maximum possible speed
- scrapy fetch "website_url": download the html file and write its contents to stdout
- scrapy settings: gets the config settings for scrapy shows project specific settings if this is called inside a project
- scrapy view "website_url": open the url in the browser the way Scrapy will "see" it. 
  - dynamic content is not rendered
- scrapy shell "website_url": interactive shell where you quickly prototype what you want to debug without building a Spider
  - crawler
  - item
  - request
  - response 
  - settings


Scrapy architecture
- scrapy engine: controls data flow and triggers all events