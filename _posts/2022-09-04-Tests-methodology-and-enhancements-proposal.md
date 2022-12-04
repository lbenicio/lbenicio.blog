---
layout: posts
title: Tests methodology and enhancements proposal
icon: fa-comment-alt
tag: scaling, ruby on rails, kubernetes, container orchestration
categories: analysis
---

Continuing our post series about rails app performance. This week we are going to see our tests methodology and enhancements proposal.

# Sections

1. [Tests methodology and enhancements proposal](#tests-methodology-and-enhancements-proposal)

2. [Conclusion](#conclusion)

# Tests methodology and enhancements proposal

By this time, the reader might already have guessed correctly; our proposed enhancements are related to cache. For this study, we are going to use Redis it as a cache server. Following Towards Scalable and Reliable In-Memory Storage System: A Case Study with Redis, we are using a similar configuration based on a five nodes cluster. The idea here is to simulate a Redis cluster with 1 master node, with 2 slave nodes and two replicas for consistency so we can achieve a much similar production level environment on our study. Each of the nodes gets 1000MB of RAM available to cache using the LRU (Least Recently Used)2 strategy. If we look at Analysis of a Least Recently Used Cache Management Policy for Web Browsers from Vijay S. Mookerjee and Yong Tan where they study this strategy for web browser caching, it perfectly fits our study case where we are displaying a commonly accessed page. This behavior happens a lot on our application. For example, on Consul, a platform for public debates is naturally expected to the page which lists the frequently accessed debates. Most of the users would be navigating from different debates and, frequently, accessing the same debates pages list to choose a new debate to participates in. The same logic applies to polls and the legislation process.

![Overview of a typical client-server structure for Redis distributed storage service from Shanshan Chen, Xiaoxin Tang, Hongwei Wang, Han Zhao and Minyi Guo.](/assets/imgs/fig-7.3.png){:class="img-fluid"}

Talking specifically about Consul, but the concept fits most of the web applications. The majority of the page is composed of static content. And, by static, we are talking about content that does not have the necessity of being life; it could be cached for a few minutes if not an entire hour, or even in the cache for as long as the application stays alive, for example, caching assets like CSS and js files. But there are still important sections of Consul that need to be live, like the forum, the comment, and discussion section; people need to interact with each other in real-time for the discussion to be productive.

![A decentralized design for Redis by using Gossip protocol from Shanshan Chen, Xiaoxin Tang, Hongwei Wang, Han Zhao and Minyi Guo.](/assets/imgs/fig-7.4.png){:class="img-fluid"}

These five static containers acting as a Redis cluster are delivered using the same host machine, using the same simulated cluster over Minikube, so, at or max capacity, we will have 26000MB and 8 cores dedicated to our entire cluster, including the rails application, the database, and the Redis cluster. If Redis it gets 5000MB at its peak capacity, it leaves Consul and the database to escalate until 21000MB of RAM. Of course, the reader might be asking what we saw earlier in this study that we lacked RAM to increase our performance, that processing power is not the issue here, and we would need more memory. Still, we actually deploy less memory to the application and the database, so what is the problem? Counter-intuitively, by giving up 6000MB of memory to our Redis cluster, we are freeing our rails application of process power and data allocation and retrieval, which leads to an increase in responses, and page load times.

Our test strategy will be composed of 2 experiments, divided into the same categories and groups of previous studies over Consul’s original code. First, we will cache only translations and i18n related queries and files; second, we will cache entire, frequently accessed pages, as previously mentioned. In a further section, the reader will notice, and we are talking about simple cache, our first strategy, and more detailed implemented cache, our second proposed analysis. The rest is simple; we are going to investigate different ranges of memory and CPU power, from 2 cores to 8 cores and from 1024MB RAM to 26000MB RAM; the only difference this time is that the cluster, and, consequently, host machine, will be shared with the application, database, and Redis cluster.

# Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)