---
title: "Memory Leak Whack a Mole"
layout: post
---

Memory leaks happen. They shouldn't. Sometimes you just can't find enough of them. This is when `monit` or `god` comes into play. On Heroku, you don't have access to these tools, so you have to do some pipe-fitting as I like to call it.

Heroku -> 
Log Stream -> 
Logging as a Service -> (watch for a pattern, e.g. "Error R14") ->
Send an API call back to your app (is authenticated? better be!) ->
API controller parses callback, chooses an action, executes it ->
sidekiq: use their API, in rake task, in the background
puma: use redis and the v3.x plugins to watch for a redis key to appear

Fix the damn leaks!