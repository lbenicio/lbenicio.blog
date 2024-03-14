---
type: "posts"
title: Exploratory analysis on Ruby on Rails application performance very memory
icon: fa-comment-alt
tags: scaling, ruby on rails, kubernetes, container orchestration
categories: ["analysis"]
toc: true
date: "2022-08-13"
---


Continuing our post series about rails app performance. This week we are going to focus on varying the amount of memory of our simulated cluster.

# 8 cores and 16000MB RAM

We are investigating the cluster performance focusing on process power only (amount of cores of the cluster), but we were limited to 8291MB of RAM. What if we increase the cluster memory? We will check out what happens when we increase the cluster memory to 16384MB and 25000MB. It is wise to remember that the server on which we perform these tests only has 26624MB RAM, so we will leave 6000MB for the OS to handle our tests without page faults, causing it to slow down its performance.

![Completed requests with 4 cores, 16384mb RAM for 60 seconds.](/assets/imgs/1-8-16.png){:class="img-fluid"}

![Completed requests with 8 cores, 16384mb RAM for 60 seconds.](/assets/imgs/2-8-16.png){:class="img-fluid"}

From the above plots, observing the curves of 1000 users and 2000 users accessing for 60 seconds, we can see that deploy more replicas is not the solution. That is the opposite; as a developer handling the server, you must understand the sweet spot to get the best performance out of your cluster. The last plot shows us that for 4 cores, our best approach is to launch one replica per core/cluster node. If we were to look at the handled jobs, we managed to handle just fine between 50% and 70% of our requests, depending on the amount. Let us make ourselves conservatives and say that we can handle 500 simultaneous users/minute, leading us to approximately 21.6 million simultaneous users/month.

As the reader may recall, we doubled our core dedicated to the cluster that all cores available on our machine to simulate the cluster. However, we still keep the same amount of memory available for double the processing power amount. This leads us to a small amount of performance increase, around 10% for 2000 users. Still, nothing too much significantly doubled the process resources, and half of the memory is available to each replica. Thus, it is interesting to notice that we could maintain stability with more replicas; for example, the peak performance was approximately 10 replicas. Let’s jump now to full cluster power, 26624MB of memory, and 8 cores. Finally, let us plot the mean response time and compare the changes that increase memory at each step.

# 8 cores and 26000MB RAM

Ok, we were able to increase the performance a little bit, but no too much, and now we are using approximately 3000MB memory for each node on our cluster, so what going wrong here? Here we face another technical issue; we are using the same machine to simulate our cluster and our test utility, which causes one tool to throttle the other; in other words, we can’t guarantee the resources needed because if we give all machine resources to our cluster, our test application won’t be able to simulate all those user sessions. And here is where and cant advance furthermore on our investigation. In our best scenario, we could handle around 700 to 1200 users simultaneously, if we took the mean, around 950 simultaneous users, leading to approximately 41 million users/month. Let us look at the mean response time; what can we say? Did it increase?

![Mean response time with 4 cores, 26624mb RAM for 60 seconds.](/assets/imgs/1-8-26.png){:class="img-fluid"}

![Mean response time with 8 cores, 26624mb RAM for 60 seconds.](/assets/imgs/2-8-26.png){:class="img-fluid"}

# Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)