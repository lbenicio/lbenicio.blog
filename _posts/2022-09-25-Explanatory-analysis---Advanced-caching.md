---
layout: posts
title: Explanatory analysis - Advanced caching
icon: fa-comment-alt
tag: scaling, ruby on rails, kubernetes, container orchestration
categories: analysis
---

Continuing our post series about rails app performance. This week we are going to see a summary of our current analysis.
# Exploratory analysis

## 2 cores and 8000MB RAM

![Mean response time with 2 cores and 9gb RAM with ramp users accessing during 60 seconds.](/assets/imgs/fig-7.33.png){:class="img-fluid"}

Accordingly to what we have been studying so far, we will focus this part of the analysis on 1 to 100 simultaneous users. Unlike the last section, where we relied on rails gem’s automatic caching to make things simpler a bug-free, we will use advance caching strategies where we select specific parts of the code in the cache. This section will use a cache of partials of views, query results, and compare to the last section results and hope to discover if it is worth increasing software complexity is given performance improvements.

![Mean response time with 2 cores and 9gb RAM with ramp users accessing during 60 seconds.](/assets/imgs/fig-7.34.png){:class="img-fluid"}

If we start by looking at the plot,7.33 we see that for 50 users, we can handle over 94% of the requests; for 25 users, this number increase by up to 98%, while 100 users keep between 86% and 94%. If the reader remembers, from the last section, on plot 7.5, we see many similar results, where the number of completed requests keeps very close to each other. Talking about response time, we should compare 7.6 and 7.35, and here we see something interesting. Here, with a more complex cache scenario, we see a higher response time than previous tests. With the increased overhead to cache given the number of requests, we could not see benefits from the requests, and with a lack of resources, we end up with less performance. And this shows us in some environments, it is not ideal for increasing cache complexity.

## 4 cores and 8000MB RAM

![Mean response time with 4 cores and 9gb RAM with ramp users accessing during 60 seconds.](/assets/imgs/fig-7.35.png){:class="img-fluid"}

With 4 cores, we are still far behind ideal process power, and as we can see on the plot 7.35 and 7.7 that, and keep on the same complete requests interval. If we compare to previous experiments with 2 cores, we see a slight shift in requests interval were on 100 users, outliers requests spot between 88% and most of the requests keeps between 92% and 96% requests, for 50 users, the number of completed requests increase up to 98%, a small improvement.

![Mean response time with 4 cores and 9gb RAM with ramp users accessing during 60 seconds.](/assets/imgs/fig-7.36.png){:class="img-fluid"}

If we pay attention to the plot,7.36 we see the same interesting behavior. We do not have enough resources to be worth advancing, and we actually see the worst performance results. Unlike previous tests, on plot 7.34, doubling process power makes us able to decrease response time peak to 3000ms, but it is still 1000ms bigger than previous experiments with basic caching.

## 8 cores and 8000MB RAM

Unlike the previous analysis, now we are at our peak process power capacity on our simulated cluster. Let us start our analysis by comparing the results here and the ones from the previous section.

![Mean response time with 8 cores and 9gb RAM with ramp users accessing during 60 seconds.](/assets/imgs/fig-7.37.png){:class="img-fluid"}

Now stop, let us stop here a moment and look at the plot 7.37 compared to 7.12. We see similar results on both experiments, which suggest that we need a requirement of at least 8 cores to even the results we see on simpler caching strategies, up to 50 simultaneous users there a 98% completed quest rate. If we do not consider the outliers, requests for 100 simultaneous users are over 94% rate.

![Mean response time with 8 cores and 9gb RAM with ramp users accessing during 60 seconds.](/assets/imgs/fig-7.38.png){:class="img-fluid"}

Plot 7.38 make it even clearer to understanding the outliers, here we see requests for 1 simultaneous user taking over 2500ms, and this is due to caching all the content on the first request, and as we have a lot of more requests to cache, we see an outlier of over 2500ms. On the other hand, by looking at our plot and excluding the outliers, we see the same 2000ms response time for all the way up to 100 users, which is similar7.15. Let us make another jump, this time into 16000MB RAM.

## 8 cores and 16000MB RAM

![Mean response time with 8 cores and 16gb RAM with ramp users accessing during 60 seconds.](/assets/imgs/fig-7.39.png){:class="img-fluid"}

Following the behavior we have been following on previous experiments, the percentage of completed responses on the plot 7.39, excluding outliers, keep above 94% for all users. If we compare to the plot 7.16 we finally see an improvement of like 1%-2%, which is frustrating, give the wide increase in complexity on this cache strategy. According to our previous research, the problem here is that the cache has been flagged as one of the main reasons behind new bug sources, and a 1% to 2% increase in the number of responses time does not seem like a scalable solution.

![Mean response time with 8 cores and 16gb RAM with ramp users accessing during 60 seconds.](/assets/imgs/fig-7.40.png){:class="img-fluid"}

By looking into the percentage of completed requests, we showed the reader that it is not worth the complexity of advanced caching when talking about the completed request number. Let us see on mean response time, start by comparing the plot,7.40 and 7.17 we see an interesting behavior; on the plot,7.17 we see the peak response time of 2500ms, and now, on the plot 7.40, the peak response time is 2000ms, which is against what we have seen so far. But the interesting here is that there are no significant improvements in mean response time; add that to the fact that there is only 1% to 2% improvement in the percentage of completed requests, we start to see signs that advance caching simply is not worth. Let us jump into our peak performance at 8 cores and 26000MB RAM to finish this analysis.


## 8 cores and 26000MB RAM

![Mean response time with 8 cores and 26gb RAM with ramp users accessing during 60 seconds.](/assets/imgs/fig-7.41.png){:class="img-fluid"}

Finally, here we are, our peak capacity, 8 cores, 26000MB, and advanced cache; this is the last step of our research. Now we will focus on 7.41 and compare against 7.18 to discover ones for all if it is worth advance caching since we already saw that the advanced-cache could be dangerous in small environments. Starting from the plot,7.41 we see the same behavior 7.18 where there is a small percentage of outliers, and the number of completed requests keeps above 96%, and for smaller batches of users, they keep around 98% and 100%, nothing new.

![Mean response time with 8 cores and 26gb RAM with ramp users accessing during 60 seconds.](/assets/imgs/fig-7.42.png){:class="img-fluid"}

Skipping to mean response time, let us take a look at 7.42 and compare against 7.19. Now on response time, we see a small difference. Before we saw a sparse distribution of requests response times, now we see a more fixed and small range for each user group. Nonetheless, we see a smaller response time; all requests keep on the 2000ms time range, which shows us that we could not lower this metric with our available hardware.

If the reader pays attention, up to 25 simultaneous users can hold a mean response time as lower as 250ms, which is fantastic. But as we escalate just up to 100 simultaneous users, which is 4x the number of simultaneous users, the mean response time jumps from 250ms to 2000ms, 8 times the initial response time. In other words, the mean response time is increased at a faster rate than a linear correlation. On the other hand, we are still under the 3 seconds rule of thumb of Google’s SEO recommendations. Now we are going to make a general analysis of the test distribution.

# Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)