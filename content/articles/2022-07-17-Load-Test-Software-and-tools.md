---
type: "posts"
title: Load Test Software
icon: fa-comment-alt
tags: testing, load
categories: ["discussion', 'load test"]
toc: true
date: "2022-07-17"
---


Hi folks, today we are going discuss load test solutions. Let us do it.

# Technical features

Our main goal in this study is to analyze and increase cluster performance for handling multiple users concurrently, I’m going to focus on loading times and requests handled. We have to remember that we are handling a monolithic software based on Ruby on Rails, containerized and deployed using Kubernetes; it is crucial that we know ahead of time how many users we can handle and the related amount of physical server we need to deploy to handle big cities such as São Paulo. We will discuss a couple of load testing software here and why I’m using Gatling to do the job. We will focus on three main characterizes of the software, developer experience, performance, and maintainability.

We need software that is easy to use on the command line for single tests, scriptable; it needs to be executed on CI (continuous Integration) pipeline, it must be open-source and perform well. There are plenty of candidates to do the job, but I’m going to focus on 5 candidates that demonstrate the good and bad qualities of each group of features. Our focus candidates are JMeter, Gatling, Locust, Drill, k6s.

Starting with active development. The most important feature an open-source software would have is to be actively developed and have an engaged contributing community, which is also marked as a prominent soft load test tool by S. Pradeep and Yogesh Kumar Sharma. If we rely on software, we can not afoot it to be poorly developed, to analyze it we gonna check the commits and releases our these tools repositories. As you can see from the chart below, most of the market software is not as active as the first quartile. JMeter witch is a giant of the industry, actively developed by Apache, a great lead followed by k6s, and Gatling is our best bet to rely on until here. If we look at the other candidates, we have Drill and Locust, but they do not look so actively maintained as the other, which is pretty critical.

Our first candidate is JMeter, but besides, it is actively developed, and Apache behind it is a good sign too; it is an old Java application. At the same time that Java is great and reliable, it is an old application, and to start a new application and use such old technology would not be the best choice; we could take advantages of the JVM (Java Virtual Machine) using newer technologies, and we don’t too advanced tools to this study. On the other hand, following adversaries seems to be pretty good for the job. Both of them are twice actively developed as their following adversaries. K6s relies on AGPLv3, which is great, and Apache 2.0, which is the Gatling license. On the other hand, Locust goes with an MIT license, which is not a good sign for our project, and he wants to keep it as open as possible. Shining at the top, besides k6s, there is Drill, AGPLv3 as well.

![Commits and releases on main repositories](/assets/imgs/commitsAndReleasesOnMainRepositories.png){:class="img-fluid"}

Talking about coding, which one of them offers better possibilities using their environments? Suppose we start again from JMeter. In that case, it is, as mentioned before, a Java application, and a big one, which here is a score down because it would require more resources to run and test our ecosystem without taking advantage of its advanced probabilities. It is not scriptable, which is pretty bad as well. K6s here goes as our best options, as written in Go, a pretty performance-efficient programming language and runtime and scriptable in JavaScript, which is great too because it is such a versatile language. Last but not least, we have Gatling, which is written in Scala and scriptable in scala as well, which is a pretty downside of it since scala does not have a big community, but it relies on the JVM as well, and it is not as big as JMeter, which makes it very efficient. Besides being written in Rust, Drill is not scriptable, taking away too many possibilities of the table. Locust takes another hit here; it is written in Python and scriptable in Python. Besides all the amazing features python has, it is too slow and could throat our tests with bigger simultaneous user simulations.

Looking for a different approach, we see that Fernando, Monserrate, and Julio go for the JVM strengths and the golden standard for testing, the JMeter. We can think of Gatling and the evolution of JMeter in terms of modern software with, smaller code base, but it still gets the advantages Java, and the JVM bring to us. Still, we can see from their research table quoted here that Gatling got the best score out of all the analyzed tools.

# User Experience

Finally, developer experience, and how good and easy is it to use all that software? All of our candidates got a command-line interface, they all have recording features to use on the browser, dynamically generated HTML report pages, Locust and k6s here display an outstanding feature which is exporting the generated reported as CSV and still got a pretty web user interface for reading and accessing the data. Besides JMeter’s age, it got a large plugin ecosystem that allows it to handle all sorts of jobs and tweaks. There is not too much to differentiate all of them.

![table comparing the diversity of software testing tools](/assets/imgs/tableComparingTheDiversirtyOfSoftwareTestingTools.png){:class="img-fluid"}

# Performance

Just before we start talking about the test results, a disclaimer first. The tests were performed on a 4-core Celeron server running Ubuntu 18.04 with 8GB RAM as the load generator machine. For the target server, it was a 4-core 4Ghz i7 iMac with 16G RAM with hyperthreading.

If we take a look at the plot below where we compare the memory usage and maximum RPS (requests per second), it is easy to see that there is a lot of tools capable of generating more traffic then JMeter, Gatling of Locust, and Drills takes a fatal hit here, showing the slowest performance possible. But, we really don’t need that much request per second. If we could generate like 200 requests/second would be over our needs for this study, our best bet tools are on their scope range. It is important to keep in mind that these numbers were generated tweaking some performances for some utilities, like k6s, Artillery, and Locust. Still, since those tweaking are available to the final user, it is OK to rely on them.

Talking about memory usage. As expected from a Java application, not to mention an old Java application, they eat all the resources we have. JMeter here uses most resources, followed by Gatling, as known as a modern version of JMeter. K6s here seems to be our best 
bet, but do not get too excited; K6s is written in Python, which has drawbacks related to CPU performance. Since our testing server used for our tests has a nice amount of RAM, we are not so concerned about memory usage and more focused on CPU performance; ranking our possibilities here would be Locust, Gatling k6s JMeter.

![Memory usage per Virtual User](/assets/imgs/memoryUsagePerVirtualUser.png){:class="img-fluid"}

Looking both, the memory usage per virtual use and memory usage per request volumes show us the same results as before. Both Java application still in the podium seems to use more resources than the other candidates’ average. On the other hand, Gating seems to be very consistent with its memory consumption and much better than JMeter, but both of them fail terribly if compared to this study’s other tools.

# Gatling

For this study, We will go with Gatling because of its active development, a company running an enterprise solution behind it, and together with the community, its reliability provided by JVM; besides its low score on performance tests, Gatling got a company behind it, a big community helping the development which makes it easy to learn new technology, if more people are using it, there are more people to help you out with something you do no understand, as also mention in Comparative Analysis of Web Platform Assessment Tools by Solange Paz1, and Jorge Bernardino. It is developed above the JVM; it is pretty easy to add the software to the CI pipeline across different computers and architecture. We can rely on that our software will behave the same and predict the results. Basically, our best bet would be Gatling, thinking about its longevity, durability, maintainability, learning curve, and performance.

JMeter, Locust, and k6s and good candidates, but JMeter is a pretty heavy software that may throat our tests due to the server capabilities. We would not need such a big ecosystem and plugins, not to mention its poor scriptable interface. K6s fails on a critical topic, it got a tiny community, and there is not too much content about it, making it difficult to learn and study already done user-cases. It is written in python, which for our purposes of simulation concurrencies users access to some URLs could lead to CPU throating and increased response times.

![Memory usage per request](/assets/imgs/memoryUsagePerRequest.png){:class="img-fluid"}

The main problem with locust is its development, which seems to be active and some
periods but dead at others; on their repository, it is possible to see periods eighteens months of no updates, it is written in Python as well, and its license is MIT. Having said all of that, from now on, our reports will be all done using Gatling, what we can see as a great candidate marked as well by Andrei Proskurin on his research for testing java software.

# Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)