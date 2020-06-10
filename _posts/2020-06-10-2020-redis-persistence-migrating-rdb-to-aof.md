---
layout: post
title: Migrating to AOF from RDB with Redis
comments: true
tags:
- Redis
- Persistence
- Migration
---

You are using Redis with RDB persistence enabled for legacy reasons, a specific
use case or simply because that's the default persistence method and you would like 
to switch to AOF persistence by default.

As a Redis user, you will most probably encounter this situation a day. You are
wondering what is the best approach to migrate from one persistence format to
another or to use both?

Unfortunately, Redis requires to restart your server to enable AOF persistence.
Here is a 4 steps process to reduce downtime:

1. Create a copy of your existing RDB backup file.
2. From a Redis client trigger the command `bgrewriteaof`. This will create
   a backup file (named by default `appendonly.aof`) using the AOF format.
3. Edit your `redis.conf` to enable AOF persistance with the `appendonly yes`
   configuration line.
4. Restart your Redis server.

