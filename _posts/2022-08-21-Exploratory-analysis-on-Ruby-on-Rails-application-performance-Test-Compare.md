---

layout: posts
title: Exploratory analysis on Ruby on Rails application performance test compare
icon: fa-comment-alt
tag: scaling, ruby on rails, kubernetes, container orchestration #no-spell-check-line
categories: analysis
toc: true
---


Continuing our post series about rails app performance. This week we are going to see a summary of our current analysis.

# Tests analysis

Now we have to split this task into 2 tasks; first, we are going to compare the results between 16GB and 26GB of memory to see what changed, separately for 4 and 8 cores, then we look at jumping from 4 to 8 cores at 26GB of memory.

Closing our analysis to 1000 and 2000 simultaneous users, for easy-of-understanding reasons, most of the requests were handled between 1500ms and 2500ms. Doubling the available memory allowed us to shift these charts slightly to the left, making it closer to 1000ms and 2000ms. Cheers, we are getting close to the optimum performance given the resources we have. Now observe the 8 core, between 16GB and 26GB of memory, the first response, for 16Gb memory, we handle most of the request from around 1000ms to 1800ms with a quarter of the requester being over 2000ms and as high as 3500ms, and again, increasing the memory shift our chart a little to the left, making the handling request response time lower. And we see a peak on 1500ms response time.

So, what do we see here? As we increase performance, we are going a little bit more user-handed but increase our response quality, making it faster for the answered users.

Talking about the changes between 4 and 8 cores, we see that our curve changed from a separate bar to more something that remembers a Normal distribution; in other words, we are better handling the same amount of users, not quite the same amount as we have seen before, like 10-20% more.

![Completed requests time with 4 cores, 26624mb RAM for 60 seconds.](/assets/imgs/1-t.png){:class="img-fluid"} #no-spell-check-line

![Completed requests time with 4 cores, 26624mb RAM for 60 seconds.](/assets/imgs/2-t.png){:class="img-fluid"} #no-spell-check-line

The above histogram summarizes how our application scale to handle those simultaneous users; at 2000 users, we can handle between 0.4 and 0.6 of the requests, and this performance increases a little bit as we double the cores, from 4 to 8, shifting from 0.3-0.5 to 0.4-0.6. On the other hand, if we compare with 16GB of memory, we had a wider range, going from 0.1 to 06; the difference here is how sparse those two distributions are on this interval.

The main problem here is that at 100 simultaneous users, we already dropped the percentage of handled requests. At 1 user, obviously, we handled 100%, but at 100 users, we dropped to something between 80% and 90% of the requests, but why is it dropping so fast? The cluster got 8 cores and 26000MB RAM for its disposal, our maximum hardware capacity, but the performance shows signals of deterioration too fast for the number of simultaneous users. Another interesting part of these 2 plot is that the proportions keep almost linear when doubling the cores from 4 to 8. If you look at the first and second plots, one can notice that at 100 simultaneous users, we handle, on both 4 and 8 cores, around 80-90% of the requests. We can conclude that the core is not throating the cluster performance, at least for 100 simultaneous users. And here it comes, the big unknown, if we look at 4 cores and 8 cores with around 16000MB RAM, around 10000MB less than these last plots, the proportion keeps itself around 80-90% of the requests, so, let’s recapitulate what happened here. We increased all available resources for our cluster; we doubled the core amount from 4 to 8 cores and increased the RAM amount from 16000MB to 26000MB, but the requested handle keeps the same on all 4 scenarios, around 80-90%. What is causing this? Let us plot a few histograms and see when we throttle 100 simultaneous users.

![Completed requests time with 8 cores, 4096mb RAM for 60 seconds.](/assets/imgs/3-t.png){:class="img-fluid"} #no-spell-check-line

![Completed requests time with 8 cores, 8192mb RAM for 60 seconds.](/assets/imgs/4-t.png){:class="img-fluid"} #no-spell-check-line

Ok, we need to make a stop here. We just plotted our cluster performance, handled requests, kept the core amount constant at 8 cores, and started lowering the RAM amount. At 4096MB RAM and 8 cores, we just dropped the percentage of handled requests with 100 simultaneous users, going from 80-90% to, on its worsts, 30%. We got a pretty big hit here, but we need to investigate; furthermore, let us keep the RAM now at 8192MB, where we know that it can handle 100 simultaneous users, and see how cores affect this response time.

![Completed requests time with 8 cores, 16384mb RAM for 60 seconds.](/assets/imgs/5-t.png){:class="img-fluid"} #no-spell-check-line

![Completed requests time with 8 cores, 26624mb RAM for 60 seconds.](/assets/imgs/6-t.png){:class="img-fluid"} #no-spell-check-line

That is interesting; if we run our cluster as low as only 2 cores, as a 2 node cluster with 1 core each node, but with 8192MB RAM, the cluster keeps it static and handles the same amount of users. We still need further investigations because we can increase the cluster cores power from 2 to 8 and increase the memory from 8192MB to 26624MB, but the handled requests keep constant around the same interval. What happens if we lower the time interval? Until now, we kept our time interval constant at 60 seconds. The last 3 plots show us that, surprisingly, the number of handled requests at 10 users kept constant around that same interval, 80-90%. We need to plot the total number of requests to see if we are hitting the maximum requests/second that we can handle.

![Standard Deviation with 8 cores, 8192mb RAM for 60 seconds.](/assets/imgs/7-t.png){:class="img-fluid"} #no-spell-check-line

![Standard Deviation with 8 cores, 16384mb RAM for 60 seconds.](/assets/imgs/8-t.png){:class="img-fluid"} #no-spell-check-line

As expected, the number of requests / second is not the problem here; we are far from its limits. So, what do we see here? The answer is simpler than the reader may expect. We are just degrading our cluster. If we want to serve over 95% of the requests, it is essential to increase our server. As we try to reach 100% requests, we need to increase the number of servers available to the cluster. And here, we will call the Pareto Principle, which states that "for many events, roughly 80% of the effects come from 20% of the causes". That is exactly what we see here; with 20%, we can handle 80% of the requests. Still, as we are moving to a better quality application, which tries to reaches 100% response tax, you will have to do the other 80% of the effort, and that is why to handle these last few requests, we need so much more servers. The closer we are to 100%, the bigger the number of nodes on the cluster.

To demonstrate what is happening here, we have to plot our user access for 60 seconds. Still, for a more thin gradient, for example, with 1, 10, 25, 50, 75, and 100 simultaneous users, so we can understand where our cluster drops from 100% request rate to 80-90% and find how our cluster behaves and handles optimum simultaneous users. We will make another stop on our analysis to take a closer look at what is happening here and its scenario. Still, for simplicity, we will keep only the 60 seconds scenario.

![Standard Deviation with 4 cores, 26624mb RAM for 60 seconds.](/assets/imgs/9-t.png){:class="img-fluid"} #no-spell-check-line

![Standard Deviation with 4 cores, 26624mb RAM for 60 seconds.](/assets/imgs/10-t.png){:class="img-fluid"} #no-spell-check-line

The last 4 plot shows us the "softening" and "clustering" of our data point since we give more resources to our cluster, it accommodates more requests, but these extra requests come with a price. The mean response time increases over 1000ms, but what is causing this? As we can observe from the last plot, double the core amount gives a significant increase in requests responded successfully. Still, double the memory, or increase it by 50%, helps accommodate more requests at a more uniform rate, instead of handling some requests fast but others giving failure completely. To summarize, memory here plays an important spot in increasing each user’s performance, allowing a uniform scenario across a bigger range of users. The cores/cluster nodes act on handling those requests; as expected, it is responsible for the processing power.

For example, by doubling the nodes handling requests, we got over 3 times the number of successful requests. Increasing memory allows the number of successful requests to increase by 3 points, mostly because of timeout requests. We had process power to handle more requests the RAM was fully used. So, increasing the available memory allows Rails to handle a little more requests. Still, our focus here is how it increases the performance for those handled users, making navigation flow softly.

![Quartiles distributions for 50, 75, 95, and 99.](/assets/imgs/11-t.png){:class="img-fluid"} #no-spell-check-line

![Quartile distributions for 50, 75, and 95.](/assets/imgs/12-t.png){:class="img-fluid"} #no-spell-check-line

The noticeable effect is that at 100 simultaneous users, we handle 100% of the request, as we increase to 100%, it sifts down to approximately 75% of then, and, ultimately, at 2000 users, we can handle only 50% of the requests but with an insanely high page load average time, on its peak, reaching over 60000ms with 2000 simultaneous users and a percentile of 99%. As we can see clearly from the above plot, 75% of the successful requests at 2000 are handled in about 1.5 seconds, which is pretty good. However, the last 24% suffers a lot with increase server load time, but still usable, and, for least, 1% of successful requests will have a page load time of over 60000ms, which is unacceptable. Here we need to conduct further investigation to find that sweet spot where our cluster handles the job.

![Quartiles distributions for 50, and 75.](/assets/imgs/13-t.png){:class="img-fluid"} #no-spell-check-line

We shall pay attention to that with 2000 simultaneous users; the 99 percentile is 40000ms and 60000ms. We have seen before considering that the average user should expect the page to load under 100ms and acceptably wait until 1000ms. Still, at 10000ms, this same user should abandon the task and think the page is broken; we are handling on a good condition around 75% of or target group, at the point that the 95 percentile shows us the timing of 2000ms to 4000ms which is considered a poor performance website but acetable by the average user. Summarizing our condition, we are handling pretty good 75% of the requests, leaving 20% with a bad experience, and the last 5% have completely abandoned us for this poor performance.

According to what we have been discussing so far, we are pretty close to handling those 2000 simultaneous users a minute; the problem is that we need to lower those page load response time, make navigation more reliable; our goal is not to increase the user load nor the amount of simultaneous users, and here we hit a giant wall, what we see is that our cluster is being throttled, we have enough CPU power to do the job but not enough memory, what happens is that the system starts to using the swap area which leads us to disk access and an increase in Read and Write operations. Recalling from the beginning of this chapter, we are using a hard drive that explains why we suddenly, at this last 1%, jump from under 10000ms to almost 60000ms. And for this, unfortunately, we will have to go sideways since we can’t increase our memory power.

# Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)
