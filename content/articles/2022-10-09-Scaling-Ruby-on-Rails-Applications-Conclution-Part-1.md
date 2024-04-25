---

type: "posts"
title: Scaling Ruby on Rails Applications Conclusion - Part 1
icon: fa-comment-alt
tags: scaling, ruby on rails, kubernetes, container orchestration
categories: ["analysis"]
toc: true
date: "2022-10-09"
type: posts
---



Continuing our post series about rails app performance. This week we are going to see a summary of our current analysis.

# Final Analysis Considerations - Part 1

We have already concluded all our experiments and wet extensively thought all of them. The raw data and the Jupyter notebook, which produce these experiments, are available to download in the appendix A. To organize the ideas behind this study and what we have accomplished so far, we will recapitulate the most important parts and how they correlate to each other.

We started our research by understanding why performance is important and why we should care about our application’s response time. Next, we jumped into fastness scales and how fast is fast enough, and what is a good response time. Then we started studying defenses between architectures for deploying a web application based off Rails; it was using Ansible script and a monolithic server against using a Kubernetes approach with horizontally scalable cluster, where we discussed the benefits of using Kubernetes against its main disadvantage, which is losing performance but gain scalability.

Talking about how fast is fast enough and response time, it is important to recall our standards. Our research stated that an acceptable mean response time would be 3000ms, and a gold standard would be 1500ms. We have discussed that we would be handling 95% of the requests and would not care about the appeared response time ad would not use the Ajax method to give an impression to the user that our response time is lower; we measured real response time and page loads.

Our workload test environment was composed of a couple of variables. The number of cores, RAM amount, replicas set, talking about cluster parameters. We ran load tests using Gatling to stress those variables, which ran ramp users access during 1, 10, and 60 seconds for batches or 1 to 2000 simultaneous users in intervals of 25 users each. We used a couple of scripts in Scala, Bash Shell, and NodeJS, and the help of Gatling software. Our tests consisted of tweaking these parameters in cluster size and load test payload. Our goal is to use cache solutions such as Redis to increase our performance time to keep inside our golden standard of 1500ms, but 3000ms is still acceptable.

To maintain a comparison base and understand which kind of stress our software would encounter, we had to do a simple analysis of Brazil’s population. Recalling what we have studied about our country city populations and governmental web portals access data, we could estimate an average expected access load. Our study has shown us that São Paulo’s platforms, Governo Aberto1 had received at its peak access, 30000 users during November of 2019, but averaging 1500 users each month. On the other hand, the federal website for retrieving population’s data, Portal Brasileiro de dados Abertos2, received at its peak 12000 users in July of 2016. Now that the reader had remembered our goal, the standard we set to achieve, and comparison bases on Brazil data access portals data, let us jump into the results.

Our statistical analysis starts by looking at Consul original code implementation but deploys through a Kubernetes stack instead of a normal monolithic virtual server. We deploy that code using the provided Docker image but with our own implementation of Kubernetes architecture. With our simulated cluster ready and our stack deploy to Kubernetes, we ran our tests using bash and Scala scripts using Gatling. Then parse generated results using other NodeJS scripts, which gave us a CSV file to summarize. The readers can find out on our repository to simulate for their own.

By analyzing the results, we have understood that Consul, as a Ruby on Rails applications, dramatically relies on the replica set and the memory for performance, with cores playing a smaller role but still needed to increases performance, but not throttling our simulated cluster capabilities. On the other hand, increasing the replica set also increases our cluster’s memory usage, which leads us to memory being the main responsible for lowering the mean response time, thus increasing our cluster performance.

Jumping into the numbers, we saw that the original code was able to handle the requests with just a small margin above our golden standard of 1500ms with an average mean response time of 2000ms for 100 simultaneous users but 500ms for cached pages loaded directly from the file system and just around 2500ms for 2000 users. It is also important to remember that most of our requests were clustered around 1500ms ranging all users intervals. But what is important is to pay attention to all plot shapes; all histograms remember a Normal distribution, where it starts with a small and increases to its peak, and then the curve smoothly decreases.

Caching translations, session data, GETs, and HEADs requests allowed us to handle 95% of the requests within our golden standard or 3000ms. But not only that, in better scenarios, we were able to lower our response time by half, allowing page loads of 500ms. If we were to talk about handled requests, our numbers jumped to almost 100%, excluding outliers due to network problems. On higher simulations, with like 3000 or 5000 simultaneous users, the numbers dropped slightly, but we were able to handle 75% of the requests within a page load time of, as low as, 500ms. Caching allows us to delivery speedy page loads; the problem is that we start to throttle with process power to handle all open connections to our machine.

The proposed improvements were all about caching and if it was worth increasing complexity to increase performance. To do so, we divided our tests into two groups; first, we analysis simple caching, just leaving Rails to cache what it "thought" was important and caching translations. We jumped into the caching page and partials to identify if performance increases and if it is worth the performance difference due to managing cache state on small pieces of code updates. It is also important to keep in mind that our research has shown us that cache is one of the main causes of bugs in enterprise level applications.

We did pretty much the same stress tests for the original consul code, one batch for simple cache strategy and another for the advanced caching. First, looking at our plotted results, we saw that there were not too many improvements to our application level. We saw improvements for handling 100% of the requests on 100 simultaneous users. The plots became more stables, but we did not see much improvement in the average mean response time; the stability came from a smaller standard deviation on the cache version than the original code.

The results discussed above are what we expected from our research. Cache helps out by eliminating some access the Rails application does to the database. This shows us that cache is an important ally in a situation with small resources. It is crucial to leave some of the resources for a cache server. It will dramatically improve your application. The other scenario is where you have huge amounts of simultaneous users on your applications and problems like store session data. It can dramatically improve by leaving those access to the cache instead of a regular SQL database.

# Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)