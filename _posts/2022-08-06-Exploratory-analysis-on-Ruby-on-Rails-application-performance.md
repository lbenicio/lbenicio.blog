---

layout: posts
title: Exploratory analysis on Ruby on Rails application performance
icon: fa-comment-alt
tag: scaling, ruby on rails, kubernetes, container orchestration
categories: analysis
toc: true
---


Hi folks, this week we are going to take a look at our experiments running a fixed amount of memory, 9000mb, but varying our core count, to understand the processor impact on our cluster simulation. Let's dive in!

# 2 cores and 9000MB RAM

![Percentage of successful requests 2 cores, 9000mb. Accessing for 1 second](/assets/imgs/2-9000.png){:class="img-fluid"}

As expected, as we increase the number of simultaneous users accessing the application, the number of answered requests decreases as low as 20%. But, let us take a look from the other side, starting with simple math. We are talking about 2000 users accessing the application for 1 second, which means we have 12000 users each minute and a total of approximately 518 million simultaneous users accessing during the month. That is too much; let’s not forget that we are using only 2 nodes on our cluster.

Let us see now how good was our response time, considering that if a request takes less than 800ms to return, it is the best scenario; if it takes between 800ms and 1200ms, we accept it as well, but if we got over 1200ms, we start to get worried.

![Response time 2 cores, 9000mb. Accessing for 1 second.](/assets/imgs/2-8-1.png){:class="img-fluid"}

As we can see from the above plots, most of the requests, which are completed, keep inside our margin for "good" response time (t < 800ms). Now forget for a moment the number of replicas we are using on our cluster; let’s check out how a 2-machine cluster could handle that amount of users accessing the application during 1 second.
As expected, we can check that we can handle not so many simultaneous user connections without computer power. Between 1-100 users/second, we can see that most of the requests were handled just fine by our cluster. We need to check out what happens if we split this user load for 10 seconds and 60 seconds.

![Completed requests with 4 cores, 8192mb RAM for 60 seconds](/assets/imgs/4-8-60.png){:class="img-fluid"}

We got a problem; we were testing with 2000 users accessing during one second, now we just moved to 2000 users accessing during 60 seconds, but as we can check on the above histograms that we only got a slight increase over the completed requests signaling us that we are reaching top performance for 2 nodes cluster, let’s see what changes if we increase our cluster size.

# 4 cores and 9000MB RAM

![Completed requests with 4 cores, 8192mb RAM during 60 seconds](/assets/imgs/p4-9.png){:class="img-fluid"}

What pop’s up to our eyes immediately? We got a bigger number of requests being accepted at the same time. Our distribution got larger, and a larger amount of requests were answered. As we will see, not all of them got good response times, but the number of answered users increased significantly. Ok, let us check now the response times.

At this point, the user should have already understood what is happening and our goal. As we increase the number of available resources (cluster nodes) dedicated to our application, we get a slight performance increase, getting more users answered correctly. Their response times get slowly better. Jumping into our machine performance, handling all those users if we increase their time available.

Let us stop here for a moment. We are focusing on users during a 60 seconds access time. But our numbers are still not ideal. Our goal is to handle as much as we can and reach as close to 2000 users for 60 seconds. As we can see from the above chart, with 4 nodes, 8192MB total available RAM to the cluster for 60 seconds, we handle almost all requests at a max of 100 simultaneous users. Let’s jump to 8 nodes.

# 8 cores and 9000MB RAM

![Completed requests with 8 cores, 8192mb RAM for 60 seconds](/assets/imgs/p8-9.png){:class="img-fluid"}

Now, we should take a special look at our data. At this point in our study, we are using 8 nodes for our cluster simulation, and as the reader might remember, that is the number of cores that our server got. Consequently, we might expect that this is as good as we can get with this server. So, let’s see how our cluster behaves with 8 nodes with 60 seconds.

Let us go step by step and take a closer look. First things, first. At 1 user, we handle 100% of the requests, but nothing new here; we are talking about just 1 user per minute. Surprisingly, at 100 users/minute, we still handle almost 100% of the requests, not all of them at our perfect timing response, but we are handling them. Considering the numbers, 100 users/minute are 144000 users/day, and approximately 4.3 million users a month if we look to a city of a population of 1 million people, as much as everyone accessing our application one time a week.

With our current setup, on a 2 nodes cluster with 8192MB RAM, we can handle 4.5 mil- lion simultaneous users a month (100 each minute) witch mean we can handle the expected population using our services three times a month, or 67% of then using it every week, but let us see if we can get better results.

But, if we remove the outliers cities, São Paulo and Rio de Janeiro, we got a population of 1 million inhabitants, which means we could handle all of them using our application once a week. Looking at 1000 simultaneous users, we can handle 80% of the requests just fine; in other words, 800 users wouldn’t notice any downtime on our serves, but 20% of them will be angry.

Considering just 800 users simultaneously, accessing for 60 seconds, we are talking about 34.5 million users a month. Handling even the most populated city of Brazil, São Paulo, using our service 3 times a month.

If we look at the access data of the Governo Aberto portal’s we see that if we handle 10 users a minute, we could handle almost 450.000 requests, which could serve just well the peak request capacity of 30.000 users/month. Here, in our example, we are using São Paulo’s Open Government data, the most populated city.

As shown from the above plot, we managed to handle almost 80% of the requests of 1000 simultaneous users but around 50% of the 2000 users, getting even worse results, which shows us that if we get an overload of our server with peak usage, we should increase the number of nodes on the cluster. In contrast, we can see that it is quite simple for 100 simultaneous users to handle the traffic. Now we are going to check what the response time for 1000 users
was?

![Mean response time with 2 cores, 8192mb RAM for 60 seconds](/assets/imgs/m-1.png){:class="img-fluid"}

![Mean response time with 4 cores, 8192mb RAM for 60 seconds](/assets/imgs/m-2.png){:class="img-fluid"}

![Mean response time with 8 cores, 8192mb RAM for 60 seconds](/assets/imgs/m-1.png){:class="img-fluid"}

As shown in the above examples, as we increase the simultaneous user access, the response time deteriorates quickly. Still, it seems to have a relation between the response time, the cluster process power, and the number of acting nodes. It is crucial to create an equilibrium between those resources, which requires further investigation.

# Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)