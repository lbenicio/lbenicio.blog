---
layout: posts
title: Tests Analysis on Improved code
icon: fa-comment-alt
tag: scaling, ruby on rails, kubernetes, container orchestration
categories: analysis
---

Continuing our post series about rails app performance. This week we are going to see a summary of our current analysis.

# Sections

1. [Tests analysis](#tests-analysis)

2. [Conclusion](#conclusion)

# Tests analysis

Before we jump into a general analysis of our case scenario, we would like to use the plot 7.20 and 7.21 to demonstrate why focus on simulations with a higher number of users is of no need for us in this study. First, let us look at the plot 7.21 we can handle the request pretty well, all of the tested groups keep under 3 seconds limit set by Google that we already mentioned. But, if we pay attention to the plot 7.20 we can spot the problem right on the way. Our request handled peak is 80% for 500 users, increasing all the way up to 2000 simultaneous users make our requests rate drops to as lower as 20%-30% which is even less than 1000 simultaneous users rate, which is between 50% and 70%. The problem here is that even on 500 simultaneous users our acceptance ratio is pretty low, we can not miss one out of 5 requests.

![Completed requests time with 8 cores, 26624mb RAM for 60 seconds for over 100 simultaneous users.](/assets/imgs/fig-7.20.png){:class="img-fluid"}

![Mean response time time with 8 cores, 26624mb RAM for 60 seconds for over 100 simultaneous users.](/assets/imgs/fig-7.21.png){:class="img-fluid"}

The reader might have noticed that we had brought to our attention that we had outliers numerous times during these plots, now we need to take a closer look into these outliers and see how sparse is these experiments. And here we will divide into 4 groups, percentile 50%, 75%, 95%, and 99%. Let us start our analysis with the 50% percentile and go all the way up to 99%.

![Response time in milliseconds for 50% requests.](/assets/imgs/fig-7.22.png){:class="img-fluid"}

If we pay attention to the figure 7.22 we can see that as we increase the number of simultaneous users, and, consequently, the raw amount of requests shooted through the server, the number of outliers increase, and, like we have seen on other plots (figures: 7.19, 7.17, 7.15, 7.8, and 7.6) we can clearly see the distribution clustering over 90% but some request close to 80% e that is out outliers that we were talking previous on this study, as we were guessing before, now we are sure that this measurement could be excluded focusing on the correct results.

![Response time in milliseconds for 75% requests.](/assets/imgs/fig-7.23.png){:class="img-fluid"}

The plot 7.23shows us exactly the problem we saw in this whole analysis, and at the same time, proves to the reader what we have been discussing. We reached 100 simultaneous users with over 95% completed requests ratio, but there are outliers, and there will always be, which is why we have seen strange results across our plots. But as we decrease the interval width, we start seeing a different behavior.

![Response time in milliseconds for 95% requests.](/assets/imgs/fig-7.24.png){:class="img-fluid"}

We need to stop here on the plot 7.24. Differently from 7.23 and 7.22 we see here on 7.24 outlier all the way up to 95 simultaneous users. Still, at 100 simultaneous users, we see a bigger interval but no outliers. Of course, we exclude 1 simultaneous user since that is not much data to be analyzed. if the reader takes a closer look at this last boxplot, we see that the upper half is sparse precisely to the median line. In contrast, the lower half is denser, backed up by outliers only on the upper half (which represents higher response times). This confirms that our small percentages of not completed requests have higher response times is due to outliers on the HTTP interface.

![Response time in milliseconds for 99% requests.](/assets/imgs/fig-7.25.png){:class="img-fluid"}


We can analyze the 95 and the 99 percentiles together as the behavior is similar. In the figure 7.25 we start to see fewer outliers as the length of the interval has sort. But, the interesting part here is to notice how the response time (y-axis) has increased. On the plot 7.22, for 50% of the requests only, the response time was 800ms. As we increase to 75%, the response time increased to 1200, which is still excellent. Going to 95%, things start to deteriorate, response time, counting the outliers is up to 3500ms, and without then, it would be 5000ms, for 100 users. And finally, for 99%, we see that the response time is up to 60000ms, or 60 seconds, which is incredibly high, and this behavior happens even on 25 simultaneous users. If the reader could pay attention to 10 simultaneous users on the figure 7.25, the reader might notice that even on 10 simultaneous users, we see responses time of over 20000ms (on outliers), and this give us a hint that no matter how big our cluster be, it will always be some requests that will break of taking longer responses time, and that is due to our protocol, we can not do anything about it.

At this point, data become very dense, so we are going to divide into 4 groups, again, at the same 4 groups, 50%, 75%, 95%, and 99% percentiles plots, printed out on figures 7.26, 7.27, 7.28, and 7.29 respectively. Let us begin with the plot7.26.

![Completed response time histogram for percentile 50%.](/assets/imgs/fig-7.26.png){:class="img-fluid"}

If we focus on the plot, 7.26we can see the same behavior we spotted earlier and confirmed our guesses that there is a small percentage of outliers on the requests. We can see that most of the requests for 100 simultaneous users land on 200ms, some on 400ms, and our outliers, about 800ms. The interesting part is that we see this behavior for 95, 90, and 75 users, look for the upper half of the plot labels. If we look to the lower half, from 10 to 50 users, the results are clustered around 100ms to 200ms, but the outliers for from 300ms to almost 500ms.

![Completed response time histogram for percentile 75%.](/assets/imgs/fig-7.27.png){:class="img-fluid"}

As we have seen on the boxplots earlier on the figure 7.22 and 7.23now on figures 7.27 and 7.26now with response time on horizontal axes. As the reader might have already spotted, for 100 users, up to 75% of the requests we handle it with an excellent response time of 100ms to 200ms, and even the outliers only increase those numbers up to 800ms.

If the reader remembers from our previous research, we should never take more than 5 seconds to load a web page because we could lose SEO ranking, and the user might leave the task behind. And here, for the 95% percentile, we just hit this limit and increase it by 40%, increase our maximum response time to almost 3500ms, which is 500ms over our desired limit.

![Completed response time histogram for percentile 95%.](/assets/imgs/fig-7.28.png){:class="img-fluid"}

![Completed response time histogram for percentile 99%.](/assets/imgs/fig-7.29.png){:class="img-fluid"}

Finally, if we pay attention to the plot 7.29, differently from all other plots, to handle 99% of the requests, we have to increase our cluster resource on an impracticable level, it is just not worth it. If we remember together, we are with 8 cores, 26000MB RAM, and citeRedis cluster disposal of our application. For over 10 simultaneous users, the response time is higher enough to make our website got a low score on the SEO index and make all of our users tired of using the service. Think about the Pareto principle, this last 1% would be worth only for big companies in technology industries, but not what most of the enterprise-level applications would have the technical or financial capacity to handle.

![Number of completed request x number of cores.](/assets/imgs/fig-7.30.png){:class="img-fluid"}

![Mean response time for completed requests x memory amount.](/assets/imgs/fig-7.31.png){:class="img-fluid"}

Before we finish this section, we would like to look at the plots’ number of completed requests and mean response time variations according to cores and RAM 7.30. As we can see from the above plots, as we increase memory and the number of cores, we reach 100% requests. On the other hand, focusing on the plot 7.31, we can see how response time decreases over resources increase. The plot 7.31 clearly shows us that around 8 cores, response time stick around 2000ms, against initial 6000ms-8000ms for 2 cores. Talking about process power and cores, we can better understate its effects by looking at the plot 7.30 where we can clearly see 1024MB of RAM running 0 requests, but as we increase memory, we reach over 90% requests. Similarly, we can spot for 8 cores, numbers of requests being dramatically impacted by RAM. Now, let us jump into advanced caching, where we will be using partials and page caching to identify if the benefits are worth the increase in complexity.

Finally, we would like to point out to 7.32 hat memory is the main responsible for lowering the mean response time. We second of that would be the replica set; as we increase the number of replicas, we cannot decrease mean response time and increase the number of handled requests. Following the number of handled requests, cores play an important role in memory, but memory is definitely one of the main ones responsible for performance.

![Correalation heatmap.](/assets/imgs/fig-7.32.png){:class="img-fluid"}

# Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)