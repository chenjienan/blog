---
title: Implementing Cache for Cloud-based Application
date: 2019-03-07 16:32:43
tags: work
category: knowledge
---

## Why is Caching?
User reads and writes data when interact with the database.

Cache is a datastore, just like the database, but cache store data in memory so it doesn't have to interact with the filesystem. Cache also store data in very simple data structures (e.g. key-value pair). 
- improve performance
- increase availability

pick the right data to be cached
- simple data
- data that is accessed often and doesn't change often

Invalidation policy
- set the right poicy (e.g.retention)
- balance between performance and consistency

pre-populate the cache (e.g. country, phone code)

## What is Azure Redis Cache?
- Cloud service designed for caching.
- provide in-memory data storage for fast data operations
- based on the open-source Redis cache
- Stores data in: strings, hashes, lists and sets
- Advanced features: 
  - Geo-replication (HA)
  - Cluster (better performance)
  - data persistence (backups)

## Up and running Azure Redis Cache
1. Create and use Azrue Cache
   check this [link](https://docs.microsoft.com/en-us/azure/azure-cache-for-redis/cache-dotnet-how-to-use-azure-redis-cache)

2. Install NuGet package (2 options)

   StackExchange.Redis [NuGet](https://www.nuget.org/packages/StackExchange.Redis/)   
   Microsoft.Extensions.Caching.Redis [NuGet](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/2.2.0)
3. Retrieve host name, ports, and access keys