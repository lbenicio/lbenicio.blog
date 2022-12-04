---
layout: posts
title: Scaling Ruby on Rails application
icon: fa-comment-alt
tag: scaling, ruby on rails, kubernetes, container orchestration
categories: analysis
---

Now i will try a series of posts discussing performance increase on a Ruby on Rails application. To do so, we are going to use [Consul](https://github.com/consul/consul) as a sample application. In this first post, we are going to see the lab environment where the tests will the performed. Each batch of tests will be released on a week basis, after all posts have been releases, i will drop the raw files.

# Sections

1. [Host machine and OS specification](#host-machine-and-os-specification)

2. [Tests methodology](#tests-methodology)

3. [Get to know the data](#get-to-know-the-data)

4. [Conclusion](#conclusion)

# Host machine and OS specification

| Device          | Class                                      |
|-----------------|--------------------------------------------|
| Architecture    | x86_64                                     |
| CPU op-mode(s)  | 32-bit, 64-bit                             |
| Byte Order      | Little Endian                              |
| Address sizes   | 36 bits physical, 48 bits virtual          |
| CPUs            | 8                                          |
| On-line CPUs    | 0-7                                        |
| Thread per core | 2                                          |
| Core per socket | 1                                          |
| Sockets         | 1                                          |
| NUME Nodes      | 1                                          |
| Vendor ID       | GenuineIntel                               |
| CPU Family      | 6                                          |
| Model           | 58                                         |
| Model name      | Intel(R) Xeon (R) CPU E3-1230 V2 @ 3.30GHz |
| Stepping        | 9                                          |
| CPU MHz         | 3637.993                                   |
| CPU Max MHz     | 3700.0000                                  |
| CPU Min MHz     | 1600.0000                                  |
| BogoMIPS        | 6585.38                                    |
| Virtualization  | VT-x                                       |
| L1d Cache       | 32k                                        |
| L1i Cache       | 32k                                        |
| L2 Cache        | 256k                                       |
| L3 Cache        | 8192k                                      |
| Flags           | fpu vme de pse tsc msr pae mce cx8 apic    |
|                 | mtrr pge mca cmov pat pse36 cl fl ush dts  |
|                 | sep acpi mmx fxsr sse2 ss ht tm pbe        |
|                 | syscall nx rdtscp lm constant_tsc          |
|                 | arch_perfmon pebs bts rep_good nopl vmx fl |
|                 | xtopology nonstop_tsc cpuid aperfmperf fl  |
|                 | pni pclmulqdq dtes64 monitor ds_cpl cl fl  |
|                 | smx est tm2 ssse cx16 xtpr pdcm pcid       |
|                 | sse4_1 sse4_2 x2apic popcnt  es tpr_shadow |
|                 | vnmi xsave avx f16c rdrand lahf_lm ept sep |
|                 | tsc_deadline_timer cpuid_fault pti dtherm  |
|                 | expriority mtrr pge mca cmov pat pse36 pln |
|                 | pts vpid fsgsbase smep erms xsaveopt ida   |
|                 | arat ush dts acpi mmx fxsr sse2 ss ht tm   |
|                 | pbe syscall nx rdtscp lm constant_tsc smx  |
|                 | arch_perfmon pebs bts rep_good nopl cpuid  |
|                 | xtopology nonstop_tsc aperfmperf pni vmx   |
|                 | pclmulqdq dtes64 monitor ds_cpl est tm2    |
|                 | ssse cx16 xtpr pdcm pcid sse4_1 sse4_2 pln |
|                 | tsc_deadline_timer es vnmi xsave avx f16c  |
|                 | cpuid_fault pti dtherm tpr_shadow x2apic   |
|                 | expriority ept vpid fsgsbase smep erms pts |
|                 | popcnt rdrand lahf_lm xsaveopt ida arat    |
| System          | Computer                                   |
| bus             | Motherboard                                |
| memory          | 31GiB System memory                        |
| processor       | Intel(R) Xeon (R) CPU-1230 V2 @ 3.30GHz    |
| bridge          | Xeon E3-1200 v2/Ivy Bridge DRAM Controller |
| bridge          | Xeon E3-1200 v2/3rd Gen Core processor     |

# Tests methodology

As we can see from the above commands, I’m working on an 8 cores machine with 31GB of RAM. I will use Minikube with default RAM (9GB) and then test it with 15GB and 25GB, leaving 6GB for the host OS.
For this study, and as we will use only one host machine with Minikube to simulate a cluster, at any moment of this document, when I’m referring to cores or nodes, I’m talking about the same thing. Each core on the host machine represents a node on a Kubernetes cluster.
I’m going to start simulating a cluster with 2 nodes and increase it by a power of 2 until it reaches 8 nodes. At the same time, we will test our cluster with 1, 2, 4, 8, 16 replicas of our application; 1, 10, 100, 1000, and 2000 users connecting during 1 second, 10 seconds, and 60 seconds.

# Get to know the data

First of all, let’s get to know what kind of data we are talking about. Check how the data is disposed of.

| Data columns                | Data Type           |
|-----------------------------|---------------------|
| id                          | 4935 non-null int64 |
| simulation_duration         | 4935 non-null int64 |
| number_of_simultaneous_users | 4935 non-null int64 |
| delayed                     | 4935 non-null int64 |
| cores                       | 4935 non-null int64 |
| memory                      | 4935 non-null int64 |
| replicas                    | 4935 non-null int64 |
| total_requests              | 4935 non-null int64 |
| total_requests_ok           | 4935 non-null int64 |
| total_requests_fail         | 4935 non-null int64 |
| t_800                       | 4935 non-null int64 |
| t_800_1200                  | 4935 non-null int64 |
| t_1200                      | 4935 non-null int64 |
| failed                      | 4935 non-null int64 |
| minResponseTime_total       | 4935 non-null int64 |
| minResponseTime_ok          | 4935 non-null int64 |
| minResponseTime_fail        | 4935 non-null int64 |
| maxResponseTime_total       | 4935 non-null int64 |
| maxResponseTime_ok          | 4935 non-null int64 |
| maxResponseTime_fail        | 4935 non-null int64 |
| meanResponseTime_total      | 4935 non-null int64 |
| meanResponseTime_ok         | 4935 non-null int64 |
| meanResponseTime_fail       | 4935 non-null int64 |
| standardDeviation_total     | 4935 non-null int64 |
| standardDeviation_ok        | 4935 non-null int64 |
| standardDeviation_fail      | 4935 non-null int64 |
| percentiles_50_total        | 4935 non-null int64 |
| percentiles_50_ok           | 4935 non-null int64 |
| percentiles_50_fail         | 4935 non-null int64 |
| percentiles_75_total        | 4935 non-null int64 |
| percentiles_75_ok           | 4935 non-null int64 |
| percentiles_75_fail         | 4935 non-null int64 |
| percentiles_95_total        | 4935 non-null int64 |
| percentiles_95_ok           | 4935 non-null int64 |
| percentiles_95_fail         | 4935 non-null int64 |
| percentiles_99_total        | 4935 non-null int64 |
| percentiles_99_ok           | 4935 non-null int64 |
| percentiles_99_fail         | 4935 non-null int64 |
|                             |                     |

Fine, we are going to focus our analysis on response time and failures. Our goal is to understand how to increase the rails application performance to handle as much as possible requests per Kubernetes pod.

# Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)
