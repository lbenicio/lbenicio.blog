---
layout: posts
title: Scaling Ruby on Rails Applications Conclusion - Part 2
icon: fa-comment-alt
tag: scaling, ruby on rails, kubernetes, container orchestration
categories: analysis
---

Continuing our post series about rails app performance. This week we are going to see a summary of our current analysis.

# Final Analysis Considerations - Part 2

On advanced caching problems, the benefits are even less worth it. If you are managing huge enterprise-level applications with thousands of simultaneous user access and a team to maintain your application, you will benefit from advanced caching. Our analysis has shown us no significant improvements when compared to the basic caching scenario. If we keep in mind that advanced caching is one of the main sources of bugs on the enterprise-level application, when can infer that caching page, partials, query results all over the code to decrease even more the database access and lower our resources is not worth the work you will have to spend on those upgrades, unless you are managing high traffic servers.

Keeping it simple, if you are low on resources, or if you are managing enterprise-level applications which does not have much traffic, it is important to keep caching on a basic level. Let automated caching utilities do the job, do not write "cache code" on your application, and already see improvements on your application. The other scenario is managing an application with 5000, 10000, or even more simultaneous users, which heavy load on databases servers. In this scenario, you must use advance caching to relieve as much query’s to the SQL databases, like PostgresSQL, as possible, as querying databases is significantly slower than access caching databases like Redis.

Utilities like actionpack-page_caching gem3, which automatic caches all GETs and HEADs requests, or automatic caching utilities given by rails itself like translations for i18n, or session data caching are the best utilities to improve your web application without increasing complexity. If you are interested in small advancements, the reader might want to try caching partials and query results for simple queries for heavily accessed pages, which could relieve database access and process power needed. Still, it is vital to keep caching simply. Advanced caching, for complex requests and actions, is dangerous because it could lead your development team to spend more time-solving bugs caused by the cache, improving your application, or solving all other bugs and issues that are already on the road-map.

Finally, it is also important to understand how you will build your Kubernetes architecture. For example, our experiments have shown us that one Postgres database instance could handle just a fine 5000 requests simultaneously, so, probably, you would want to use 2 or more instances or your Rails application to connect to the same Postgres database. Another interesting understanding is how you will build your Redis cluster and your Rails replicas connected to the same cluster. For example, how much data are you storing, how many Redis instances would satisfy your Rails cluster’s needs? You should not build a Kubernetes cluster with 1-1-1 instances, 1 rails replicas for 1 Postgres replica, and 1 Redis replica. To maintain instability, we have seen that it is important to keep a Redis cluster of at least 5 nodes for replicas and master-slave behavior, and, as Redis is using LRU strategy, if you leave small amounts of memory available for caching, you will experience a huge amount of page faults, due to caching being constantly swapped, which will leave on the worst performance then it was without caching.

Think about the replica set; it is better to leave Kubernetes to manage the replica set; you should leave threshold based on CPU and memory for when the container reaches 75% of CPU usage or 75% of memory usage it should escalate the replica set, then if the memory of CPU is as low as 25% the cluster should decrease the replica set. To take advantage of horizontal escalators’ full potential, you should use a host solution that proved automatic cluster management that can add or remove machines to your cluster, allowing Kubernetes to increase its power even more or decrease your costs application as needed.

# Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)