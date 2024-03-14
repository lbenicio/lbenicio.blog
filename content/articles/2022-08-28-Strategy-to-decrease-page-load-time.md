---
type: "posts"
title: Proposed Improvements on Ruby on Rails application performance test compare
icon: fa-comment-alt
tags: scaling, ruby on rails, kubernetes, container orchestration
categories: ["analysis"]
toc: true
date: "2022-08-28"
---


Continuing our post series about rails app performance. This week we are going to see a summary of our proposed improvements.

# Strategy to decrease page load time

As we are trying to deliver data as fast as possible to increase a web application’s performance, our goal is simple: cache. We are going to implement some caching on Consul on some levels. First, we are going to cache only translations, i18n1 related queries. Our second approach is more aggressive. This time, we will cache an entire page to see how it affects the performance since some pages like your website’s index are frequently accessed. Sometimes, your application has to make hundreds of access to the database to get the information required to deploy an important and frequently accessed page. This seems to be a clear sign of a great fit for caching.

Accordingly to Instant Redis Optimization How to from Arun Chinnachamy the great advantage that the NoSQL database brought to the industry was fast data access. The purpose of using NoSQL, and the NoSQL movement that started to grow was the fast data access, and again, it Redis is the leading NoSQL database for fast data in the industry. Still, it is worth mentioning the options, which are Cassandra, MongoDB, Riak, CauchBase, Memcached. Another interesting study about caching tools performance is Performance Comparison between Five NoSQL Databases from EnqingTang, and Fan which
focus their analysis on Redis, MongoDB, CauchBase, Cassandra, HBase. In their study, they highlight exactly what we are trying to investigate in this study. In the era of Big Data, mobile internet, and a huge amount of devices collecting data that need to be processed and disposed of to the client queries, performance becomes insentient. That is where NoSQL and cache come into play. In our scenario, we apply the same content on frequently accessed pages, which is a perfect scenario for caching. It can release the processing power required to deliver translations for static text or the whole page, if necessary.

Another interesting analysis is NoSQL Databases: Critical Analysis and Comparison from Adity Gupta, Swati Tyagi, Nupur Panwar, Shelly Sachdeva and Upaang Saxena. The NoSQL model differences are document, wide columns, Graph, and single shared operation (where Redis shines. They also brought attention to the CAP theorem, named Brewer’s theorem after computer scientists elaborated on Brewer’s conjecture and the feasibility of consistent, available, partition-tolerant web services. We would like to highlight the figure 7.1 that emphasizes the differences and advantages of each data storage model.

![Different NoSQL databases on basis of Non-functional features from from Adity Gupta, Swati Tyagi, Nupur Panwar, Shelly Sachdeva and Upaang Saxena.](/assets/imgs/fig-7.1.png){:class="img-fluid"}

Quoting, "Cap Theorems states that it is impossible for a distributed data store to simultaneously provide more than two out of the following three guarantees:"

1. "Consistency: Every read receives the most recent write or an error."

2. "Availability: Every request receives a (non-error) response, without the guarantee that it contains the most recent write."

3. "Partition tolerance: The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes."

And that is where Redis used together with the default Postgres database used on Consul makes the difference. The standardized relational database provides all the benefits of ACID (which the reader can get a deeper understanding of reading Takai et al.) where Redis enable us to deliver fast read operations. On other words, Redis provides Availability where Postgres provides Consistency.

Finally, we want to cite Shanshan Chen, Xiaoxin Tang, Hongwei Wang, Han Zhao and Minyi Guo that wrote Towards Scalable and Reliable In-Memory Storage System: A Case Study with Redis, which presents an extended analysis on Redis as fast in-memory storage access for web applications. On their analysis, it is proposed to create a "client-node" connection on the Redis cluster, on which the client connects directly to the correct Redis node where the correct data is stored, and accordingly to their article, they were able to increase the performance of the cached application by 2 times. Another interesting proposed strategy is a Master-slave Semi Synchronization over TCP, where it improves the consistency of Redis slaves/master data and performance by 5%.

![Using Application-level Caching to Decrease the Application The workload from Jhonny Mertz and Ingrid Nunes.](/assets/imgs/fig-7.2.png){:class="img-fluid"}

We can see a diversity of studies relating the benefits of caching for application performance if we follow the study A Qualitative Study of Application-level Caching by Jhonny Mertz and Ingrid Nunes, we see a different approach, the authors have analyzed scenarios of application caching, ranging from automatic aching techniques, manual caching, background work caching, and the benefits of one or the other. For example, talking about background work caching we mean caching HTTP requests so the process checks for a cached requests before send the requests to the controller, as we can see in figure 7.2 extracted from Jhonny Mertz and Ingrid Nunes article. Accordingly to the same study, over 80% of problems related to cache on famous open sources projects, such as ownCloud, Spree, Shopizer, OpenCart, Pencilblue, S1, Discourse, Open-MRS, PrestaShop, and Open edX are related to design and implementation (when grouped in categories), so what we can conclude from this study is that caching implementation is hard. Still, maintenance is easy and is a great starting point. Most of the time, enterprise-level solution for applications uses automated cache libraries which will prevent over 80% of the problems, and, in some cases, exceed 90% of the cache-related problems.

Looking for a more complex application like PWAs (Progressive Web Applications) that are normally built on top of the same HTML, CSS, and JavaScript technologies analyzed on Evaluating the Impact of Caching on the Energy Consumption and Performance of Progressive Web Apps by Ivano Malavolta, Katerina Chinnappan, Lukas Jasmontas, Sarthak Gupta and Kaveh Ali Karam Soltany. Skipping the energy consumption analysis, we can benefit from its performance analysis of PWAs from a cache that we can dramatically benefit from caching strategies where we see load items dropping from over 4 seconds to almost 0 seconds, which, from the user point of view, is instant.

The reader might be asking why Redis, and why cache, and before we were starting digging into numbers, let us go first to see why Redis can improve our applications exponentially. Accordingly to Thiao Macedo and Fred Oliveira that wrote Redis Cookbook, we should be using Redis to optimize different kinds of applications, shown in their book for different languages and scenarios. If we go even further, at rails 5.2 implementation, they added Redis Store built in the Ruby on Rails implementation thought the ActiveStorage announced by "dhh" on rails development blog.

# Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)