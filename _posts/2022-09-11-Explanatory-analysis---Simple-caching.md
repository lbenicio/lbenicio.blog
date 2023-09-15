---
layout: posts
title: Explanatory analysis - Simple caching
icon: fa-comment-alt
tag: scaling, ruby on rails, kubernetes, container orchestration
categories: analysis
---

Continuing our post series about rails app performance. This week we are going to see a summary of our current analysis with Explanatory analysis - Simple caching.

# Explanatory analysis - Simple caching

## 2 cores and 8000MB RAM

![Completed requests with 2 cores and 8 GB RAM with ramp users accessing during 60 seconds.](/assets/imgs/fig-7.5.png){:class="img-fluid"}

Let us stop here for a moment. We have a 2 cores cluster, only 9000MB dedicated to running the simulations, and ramp users access during 60 seconds. The reader might remember from the last chapter that we will focus on or analyze one to one hundred simultaneous users because, after that, we start to throttle the host machine. If we were to look at the whole range until 2000 users, we would STILL be trapped on the same problem as before, 80-90% of handled requests; let us keep a closer look at a smaller range of users, focus on 1 to 100. On the above chart figure7.5, we can clearly see that our requests keep inside a range of 86% to 100% requests handled; what is interesting is that with 50 simultaneous users, we can keep up to 94% requests handled, keeping our application on a 5% failure acceptance ratio, but remember, we are running with 9000MB and 2 cores only. Before we scale up to 4 cores, we need to confer the response time on such hard environment conditions.

![Mean response time with 2 cores and 8 GB RAM with ramp users accessing during 60 seconds.](/assets/imgs/fig-7.6.png){:class="img-fluid"}

Looking at the mean response time on the figure 7.6, we see two different situations. The first one is that we got outlier 1 second response time measurements. Second, we got lower responses time because a smaller number of requests were handled. Still, the one that was got better responses time; in other words, the users who were lucky to get a request handled by the server was able to navigate faster, but the number of failures is large enough to make your user abandon the task, as we have seen previously on "how long a user waits for a website." This is a clear example of a throttle system so the reader could get a comparison base for core improvements that may affect our performance. Our system clearly benefits from simple caching strategies, but we still can not fully picture the benefit and downside because our cluster is throttling. Let us scale up the processing power to 4 cores, still keeping the 9000MB of RAM, to see how it affects our performance.

## 4 cores and 8000MB RAM

![Completed requests with 4 cores and 8 GB RAM with ramp users accessing during 60 seconds.](/assets/imgs/fig-7.7.png){:class="img-fluid"}

We just doubled the cluster process power from 2 to 4 cores (still half of our total size), and we got 96% handled requests for 50 simultaneous users for 60 seconds. Jumped from 86%-94% almost uniformly distributed for 100 simultaneous users to 94% completed requests with a small fraction of the requests distributed on a range of 86%-94%. The most interesting part is that we are at 9000MB RAM, and or Redis cluster uses up to 6000MB of that same memory. We need to keep investigating, but the first signal is that relieving process power and dedicating some memory for caching dramatically improve the application’s performance. As our previous research has shown us, a cache is indeed a great tool to improve performance, and, as mentioned before, we are trying to keep it simple to avoid caching bugs or cause more problems then solves which the number one problem when we are talking about cache, as our previous research have shown us, caching adding another level of complexity to an application that needs to be managed by developers.

![Mean response time with 4 cores and 8 GB RAM with ramp users accessing during 60 seconds.](/assets/imgs/fig-7.8.png){:class="img-fluid"}

As seen before, we have got a great overall performance with 4 cores for 100 simultaneous users. In the worst scenario, we could lower our max mean page load time to 3000 ms, which is 2000 ms lower than the previous iteration with 2 cores, as the reader can see in the figure 7.8. That is time to double to process power again and see what happens. But look what is happening here, except for the minimal amount of users, like below 25. We see almost a normal distribution here, as the number of users increases during a small period of time, accessing and using the same resources, cache stat acting. We got the performance improvements we were talking about, and we just hit the 3 seconds page load time that was quoted from Google’s research early on this study for 100 simultaneous users.

Before we jump into our top process power capacity, let us look at the first half of the chart figure 7.8, considering that RAM is our main issue here. We are on a small cluster, we were able to deliver great response time up to 25 simultaneous users, keep all of then under 1000 ms and the reader might want to look again at the figure;7.7, we were able to deliver such good response time to 99% of those users which is as perfect as it is possible to if we take into considerations occasional failure due he network itself.

Ok, we are sure the improvements are great, but what have we achieved so far? How better it is from 2 cores? Did we get actual improvements Doubling of performance? This is a simple question; let us compare both plots. First, let us look at figures 7.7 and 7.7 show us the completed performance, and we can see that the performance keeps static at 98%- 99%, but what about response time? Looking for figures 7.8 and 7.8 this time we see great improvements, jumping from the mean response time of 3000 ms to as low as 500 ms mean response time, which is a 6-time improvement in performance.

## 8 cores and 8000MB RAM

![Completed requests with 8 cores and 8 GB RAM with ramp users accessing during 1 second.](/assets/imgs/fig-7.9.png){:class="img-fluid"}

![Completed requests with 8 cores and 8 GB RAM with ramp users accessing during 10 seconds.](/assets/imgs/fig-7.10.png){:class="img-fluid"}

This is our top process power capacity. We will be increasing only the RAM to investigate its performance, which is also, until now, what is most affecting response time counteracting cores power, which improves the number of requests we can handle. Now, look at the figure 7.9, but it makes it a little hard to analyze such a plot, so let us concentrate on the figure 7.10. Now that we completed the group with the outlier, we can see that we could handle most of the requests at almost 92% of them for 100 simultaneous users. Interestingly, with up to 75 simultaneous users, we were able to deliver over 95% of the requests. It is about 1 second time period, just as a comparison base for further analysis with increasing memory.

![Completed requests with 8 cores and 8 GB RAM with ramp users accessing during 10 seconds.](/assets/imgs/fig-7.11.png){:class="img-fluid"}

In the figure, 7.11 we see a plot for ram user access during 10 seconds, and on the figure 7.12 the same tests but for a period of 60 seconds. We can clearly see that for both plots, figures 7.11 and 7.12 we handled over 90% of the requests for most users, and that increase the time duration increase from 1 second to 10, and then 60 seconds have shifted our comparison point slightly. As the readers might want to check for themselves, comparing both plots, we see a more concentrated number of users over 90% when we look at the 60 seconds chart. Looking for those 3 plots, we see that, as expected, when we increase our time range, we can accommodate more simultaneous users, and it is not a process burst power needed o a shot period of time. Still, a well-distributed range of users accesses a long period o time.

![Completed requests with 8 cores and 8 GB RAM with ramp users accessing during 60 seconds.](/assets/imgs/fig-7.12.png){:class="img-fluid"}

The problem with those burst access like on figure 7.11 is that simulating 100 users accessing a web application during 10 seconds is the same as 600 users accessing the same application during 60 seconds and see this heavy payload on our servers that the just dropped our capacity of handling simultaneous users at the tax rate of over 99% to 75 users to less than 95% to 25 users, and that is the same reason we have a wider group of requests closer to 82%. If we were to consider the plot on the figure,7.9 we could clearly see our outlier metrics justified for such a short period of time since we are simulating all those users accessing the application at the same second, removing it, and looking at the plot on the figure 7.10 we clearly see one-second burst access break a little bit more requests but the overall performance, when talking about the number of handled requests, keeps the same, but the performance related to responses times drops in fast.

But before we jump into response time, let us remember the plots on figures 7.5, 7.7, and 7.12 which representing the number of completed requests for 2, 4, and 8 cores, respectively. We jumped from poor numbers related to handled requests and in some cases handling only 65% of them, but all the way up to 8 cores we were able to tweak those number into higher percentages, going up to 92% of the users for 90 simultaneous users and over 86% for our ideal goal of 100 users.

Now let us talk about response time and how those are affected by an increase in the time range, from 1 to 60 seconds and, of course, comparing with 4 and 2 cores. We will focus on the plots on figures 7.13, 7.14, and 7.15 which measure response times for ramp user access during 1, 10, and 60 seconds.

![Mean response time with 8 cores and 8 GB RAM with ramp users accessing during 1 second.](/assets/imgs/fig-7.13.png){:class="img-fluid"}

![Mean response time with 8 cores and 8 GB RAM with ramp users accessing during 10 seconds.](/assets/imgs/fig-7.14.png){:class="img-fluid"}

As e have seen before, lower our time interval has a small impact on the number of requests handled and, as the reader is about to check for his own here, those timestamps get a small impact, as well, on response time measurements. Start looking for the plot,7.13 we see pretty poor performance, response time going up to 2500 ms distributed all over the spectrum with lower amounts of simultaneous users getting better response time, and as we increase the number of simultaneous users, those same requests increase their duration to over 2000 ms and almost 2500 ms.

![Mean response time with 8 cores and 8 GB RAM with ramp users accessing during 60 seconds.](/assets/imgs/fig-7.15.png){:class="img-fluid"}

Before we make another performance level jump, the plot 7.15 tips us of the behavior we saw before. If Consul original code, we reach a level we see a softening effect where our cluster starts to fit our users better. We start seeing more uniformly distributed access plots. From the last plot we see we can keep 50 simultaneous users under 1000 ms, 75 users under 1500 ms, and, of course, keep all those 100 users under Google’s 3-second rule of thumb mentioned here many times. However, we are racing to under 1000 ms performance, and we still can triple our memory capacity.

Keep on our exercise of backtracking, look at figures 7.6, 7.8, and 7.15, for 2, 4, and 8 cores respectively we see that we start with a peak performance under 5000 ms, which is good but was not good enough. When we jumped to 4 cores, we lowered those numbers by the entire 2000 ms, going to 3000 ms at worst performance, meaning we improve our response time by 60%. Then we doubled again, this time to 8 cores, but, now our performance just lowered to 2500 ms at the worst-case scenario, and again, as before, those last stage of performance seems to be the hardest to achieve, remembering me of Pareto principle, where the last 20% of performance improvements will take 80% of the effort.

## 8 cores and 16000MB RAM

We are almost reaching our host machine top capacity for simulating our cluster. Let us jump into 16000MB of RAM and 8 cores with 5 nodes Redis cluster. Until now, the whole analysis was focused on process power and the number of cores; in our simulation scenarios, cluster nodes, as known as each core simulating a single core machine running one or multiple containers. As the reader might remember, we could see some response time improvement as we increase the processing power. However, it is still important to remember the processing power is mostly responsible for the number of completed requests; the RAM is responsible for response times. having said that, we are now going to shift our analysis’s target by seeing more improvements in response times than the number of completed requests.

![Completed requests with 8 cores and 16 GB RAM with ramp users accessing during 60 seconds.](/assets/imgs/fig-7.16.png){:class="img-fluid"}

Now let us look at the figure;7.16, we have a great overall performance of up to 90 simultaneous users to deliver over 95% of the requests. The interesting part is that we see most of 100 users distribution over 94% requests ratio with its peak concentration at 97%. Still, we can clearly see a small fraction of our requests between 80% and 85%, which is intriguing and need further investigation.

![Mean response time with 8 cores and 16 GB RAM with ramp users accessing during 60 seconds.](/assets/imgs/fig-7.17.png){:class="img-fluid"}

On the other hand, focusing on mean response time, on the figure,7.17 we see that in the worst-case scenario, we are still on 2500ms. Comparing the results we see here to the ones shown on the plot 7.15 immediately brings to our attention that the performance did not increase. What is interesting that we see here is that we double the memory. Still, the chart just bounced around its mean; it is like the extra 8000MB of memory could ear us just a few milliseconds, and its event 1% shift in completed requests improvements. It is like we are reaching some kind of roof where we can go through, which is a signal that we are throttling or need much more power to gain those few extra percentages.

Before we make another performance jump, we want to take a closer look at plots 7.17 and 7.15, comparing each group of users. Starting from 10 simultaneous users, we see that the distribution kept almost the same, under 500 ms but with a small group around 800 ms. At 25 users, we can keep under 500 ms witch is the same as the 1000 ms limit for the plot 7.15. Going up to 50 users is our limit under 2 seconds limit, but we still see the same pattern repeatedly. With 75 simultaneous users, things start to change, we still keep all requests under 2000 ms with 16000MB of RAM, but at 8000MB, we see a slight fraction of the requests above the 2000ms threshold. Over 90 users start to face problems, the distribution range goes from 1800 ms to 2500 ms on both plots, but they are more uniformly distributed in the figure 7.15.

## 8 cores and 26000MB RAM

Finally, here we are, 2600MB RAM and 8 cores with 5 nodes Redis cluster with 6000MB RAM, this is our peak cluster performance, so our analysis on basic cache reaches our best scenario. Further in this study, the reader will encounter an analysis of advanced caching where we will cache specifics parts, pages, or routes of the application and compare the performance gains versus the increase in complexity add to this granularity cache strategy.

![Completed requests with 8 cores and 26 GB RAM with ramp users accessing during 60 seconds.](/assets/imgs/fig-7.18.png){:class="img-fluid"}

Let us start our analysis with a plot 7.18 and compare it with our previous results from plots 7.16, and 7.12. looking at the above, we see improvements, but soft ones. Let me explain; looking at the figure 7.18 we see that for up to 50, we handled 100% of the requests; at 75 users, we see some outliers around 94% to 98% of the requests; we see a pretty interesting thing here, our target group, 100 simultaneous users, just shift around 95%, entering our 5% acceptance ratio.

As we can see, different from the last step where we doubled the RAM amount from 8000MB to 1600MB, this time, increase another 10000MB of RAM we can handle all those users, we still have some outliers but at 100 simultaneous users, we tests are doing over 26000 requests during this time period which make it practically impossible not to have any timeouts of outliers requests due to multiple factors like routing.

The difference that we see here in the figure 7.18 is small. Before, on the figure 7.16, and 7.12 we saw a significant amount of request around 80% to 90% percent, and doubling the memory power, as we have seen before, was not enough to show us great performance improvements, which was rather frustrating. Still, here we see just a small fraction of 100 user distribution landing on lower requests, and we can safely assume us on inside our safe limits. Those requests out the cluster around 95% are outliers, and we will see further investigation on this outlier behavior in the next section.

![Mean response time with 8 cores and 26 GB RAM with ramp users accessing during 60 seconds.](/assets/imgs/fig-7.19.png){:class="img-fluid"}

We need to take a closer look at the mean response chart is plotted in the figure 7.19. Starting from 10 users, here we see no much difference between 26000MB (fig. 7.19), 16000MB (fig. 7.17), and 8000MB (fig. 7.15). The same behavior applies to 25 simultaneous users, which leads us to one conclusion, to those levels we reach peak performance and no matter how we increase our cluster we will still see those same results. The interesting is that we keep seeing some outliers where, even at 50 simultaneous users, we can see some requests lasting longer than 1500 ms, which is an incredibly high wait time for such a small number of users with that processing power, which also explains why at 100 simultaneous users we see some requests seems not being handled.

At 75 users, we see slight improvements, our mean response time distribution peaks migrate from 1000 ms - 11000 ms to 900 ms, but as seen before, it is not a great difference and could be due to small environmental changes. On 95 users, we keep on the same behavior across the 3 levels of RAM, 7.15, 7.17, and 7.19. Going to the border of our study case, at 90, 95, and 100 users, we face the same problem as we did on the section before with 16000MB of RAM; the increased amount is not large enough to allow us to see bigger differences here. Still, it shows us that to increase users’ last gap; we have to increase the effort by doing a more granular cache strategy.

If it is worth adding another level of complexity to the application by doing manual caching for partials and pages, routes, or requests, which is the main problem of caching? This is what we will be investigating in section 7.4. Until now ,we were trying to keep things simple and avoid adding another layer, which will certainly bring more bugs to our application and add more needing maintenance, so we kept only automatic caching and translations caching. But before we advance on manual caching, let us take a step back and look from afar into our simulations. We need to get an overall understanding of these experiments and this time focusing on the global conditions.

# Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)