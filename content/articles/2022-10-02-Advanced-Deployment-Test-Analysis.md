---

type: "posts"
title: Test Analysis - Advanced caching
icon: fa-comment-alt
tags: scaling, ruby on rails, kubernetes, container orchestration
categories: ["analysis"]
toc: true
date: "2022-10-02"
type: posts
---



Continuing our post series about rails app performance. This week we are going to see a summary of our current analysis.

# Tests analysis

Comparing box plot 7.43 a and 7.22, referring for percentile 50% of the requests, we see the same behavior and the same mean response time interval. Figures 7.43 b and 7.23 as on box plot for 50% show the exact time interval and outliers groups. As the reader might already expect, the same behavior happens when we are talking about 95% of the requests on plots 7.43 c and 7.2499 on plots 7.43 d and 7.25.

Now we will look at the bar plot of completed requests and mean response time to see if we can spot any differences. By looking into 7.44, more precisely, on 7.44 a and comparing against,7.31 we see, again, the same mean response time intervals and outliers, which show us that advance caching has proven itself not to be worth the complexity. The reader might be wondering if it is the answer to all scenarios, and be sure that it is not; the case we see here is that we are already at or lower response time as possible given our hardware.

![Response time in milliseconds for 50% requests.](/assets/imgs/fig-7.43-1.png){:class="img-fluid"}

![Response time in milliseconds for 75% requests.](/assets/imgs/fig-7.43-2.png){:class="img-fluid"}

![Response time in milliseconds for 95% requests.](/assets/imgs/fig-7.43-3.png){:class="img-fluid"}

![Response time in milliseconds for 99% requests.](/assets/imgs/fig-7.43-4.png){:class="img-fluid"}

![Mean response time for completed.](/assets/imgs/fig-7.44.png){:class="img-fluid"}

![Mean response time for completed requests x amount of completed requests requests x memory amount.](/assets/imgs/fig-7.44-2.png){:class="img-fluid"}

![Correlation heat map.](/assets/imgs/fig-7.45.png){:class="img-fluid"}


By looking at heat map 7.45, we see a few interesting correlations, first for replicas and number of handled requests and memory and mean response, as we have been guessing until now. On the other hand, process power and cores apparently do not have so much influence on all its results, but the problem here is that our data set is limited from 1 to 100 simultaneous users. if we compare previous 7.32 from basic caching, we see cores have less impact on response times, but there is almost the same influence on total requests, as we have already mentioned.

# Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)