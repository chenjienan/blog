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

- spiders: classes written by the user.
  - define how to parse responses and extract items
  - define what to crawl, how to crawl and how to extract data
- scheduler: reserves requests
- downloader: fetches the URL from the internet and sends it back to the Engine
- item pipelines: use for cleanning, validation, persisting to database etc.
  - items and processors: allow us to extract a logical subset of information for scraped sites
  - item pipelines: allow chaining of data transformations

Scrapy selector

specification of what HTML elements ought to be selected from processing. Scrapy supports XPath and CSS selectors.

- XPath: select nodes in an document (HTML)
- CSS: select HTML element (associate styles with them)

Spiders

- what to crawl

  ``` python
  start_requests()
  ```

- How to crawl: callback function inputs web page and outputs Items, Requests etc.
- How to parsse: selectors which determine which parts of web page are processed

create a project

``` python
scrapy startproject <project_name>
```

Spider:

- running spiders to crawl websites
- link extraction rules
- input processors
- item loader
- item pipelines




Built-in features

- Logging
- Email Notifications
- debug using telnet
- Borad crawl: crawls multiple websites concurrently at scale
  - auto-throttle mechanism
  - Graph Traversal
    - borad crawl (BFS): crawl a forest of dmains, stopping only when resources exhausted - search engines
    - focused crawls (DFS): crawl a specific site, usually with single Scrapy Spider
