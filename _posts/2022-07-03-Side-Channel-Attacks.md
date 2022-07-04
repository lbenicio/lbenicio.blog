---
layout: posts
title: Side Channel Attacks
icon: fa-comment-alt
tag: terminal, customization, theming, tutorial
categories: tutorial
---

This work briefly discusses some methods that I consider critical for the security of today's computing systems. For a better understanding of the discussion proposed here, the excerpt is divided into two sections.

In the first section we will discuss what a side channel attack is, how they arise, their scope, and why it is so difficult to mitigate side channel attacks. We present the principles behind attack methods and show that, for the most part, they are intrinsic to the system architecture of the system. We discussed a bit about attack classes and their variations.

In the second section we delve into a slightly more in-depth discussion of two attack methods, and for that we have divided it again into two subsections. First we discuss about information leakage from web applications and in the second section we discuss access to sensitive data in a virtualized environment between two virtual machines.

Leakage of web applications due to their state transitions where they use the fact that modern systems are based on the web and depend on the communication between the client and the server to transition between the different states of the application, thus allowing an attacker to use the various transitions of states to guess the current state of the application and its data.

We also discussed another attack method where the hacker uses a spy process running on a certain core to discover the encryption key used by a third process that is running on a different core, thus allowing spy processes on a virtual machine of a " stack" from a cloud provider can access some sensitive data from another virtual machine. Remembering that the process is possible in any encryption algorithm that is symmetric, as well as AES.

# Sections

1. [What is a side channel attack](#what-is-a-side-channel-attack)

2. [Scope of attacks](#scope-of-attacks)

3. [Attack classes](#attack-classes)

4. [Types of Attacks](#types-of-attacks)

5. [How to infer sensitive data](#how-to-infer-sensitive-data)

6. [Preventions](#preventions)

7. [Conclusion](#conclusion)

# Introduction

## What is a side channel attack

First of all, we need to define what we are going to talk about in this monograph. "Side Channels Attacks", or Attacks by Side Channels.

Side channel attacks are any type of attack where information was obtained through side information channels, that is, through the computer system itself and not, as the popular imagination usually believes, through a security breach or error. implementation of some algorithm which the software in question uses.

## Scope of attacks

Such an attack can be carried out by measuring system process times, energy consumption, electromagnetism or even sound (for example, when using a bank terminal and entering your password, the ATM emits specific sounds for each number.

It can vary in spectrum of technical level as well. From attacks that need to use functions in the "firmware" of a system or just simpler tactics such as listening to the sounds emitted by a numeric password keypad.

It is worth mentioning that attacks where the hacker uses social engineering to obtain data from the victim are not considered side channel attacks.

## Attack classes

In general, we can define some classes of side channel attacks.

1. Cache attack

In a shared environment such as a home computer, a "cloud" server, the attacker uses cache access monitoring tools to gain information about the victim's system.

2. Timing attack

Here the attacker measures the time of various types of computation on the victim's computer and on the victim's computer itself, analyzes the data to obtain a probable value of the contents of the victim's data.

3. Power-monitoring attack

Similar to the "Timing attack", the "Power-monitoring attack", uses tools on the victim's own computer to monitor power consumption during some computation and uses this data to cross-check with executions of the same computation on a known computer.

4. Electromagnetic attack

Similar to the "Timing attack" and the "Power-monitoring attack", the electromagnetic radiation emitted by the computer is now monitored to obtain the desired information. Here, it is worth mentioning that the attacker is often able to access data in plain text, without encryption.

5. Acoustic cryptanalysis

As in the last three classes of attack, here the attacker monitors the sounds emitted by the victim's computer during a computation.

6. Differential fault analysis

Now the strategy is to obtain the desired information by introducing flaws in the victim's computer in order to analyze the generated data.

7. Data remanence

As its name suggests, the tactic used here is actually quite simple. It analyzes the remaining data of some computation that supposedly should have been erased by the system after the end of the computation in question.

8. Software-initiated fault attacks

"Software-initiated fault attacks" are the rarest class of side channel attacks to observe. Here the method used is to change memory that was not directly allocated to the attacking process. By frequently accessing adjacent memory, the attacking process gains access to a region of memory that the operating system has allocated to a third process.

9. Optical

Here we have the simplest attack class of all, where the attacker only uses a high resolution camera to record the information present in standard data input and output devices, such as the monitor and keyboard.

In the next chapter we will analyze in more depth some more advanced tactics of attacks from the sides and directly related to the daily use of the Internet user, one referring to AES encryption keys in virtual machines and another related to access to web applications.

# Types of Attacks

## Leaking web applications

With the advancement of computers and the speed of the internet, society was migrating from a world where they built desktop applications that were sufficient in themselves, that is, they worked only with the information present there in the user's machine to another concept, where applications are web-based and consist of two parts, the server, known as the "backend", and the client side, the "frontend".

This new organization of applications made the activity of software maintenance, delivery of updates much easier, but it brought an intrinsic problem to the web architecture, the data is transiting through all computers, in an open way (even if they are encrypted, we can still read the encrypted data that is transiting the network).

## Definition

The attacker's objective here is to obtain information about the victim through the communication process between the browser on the client side and the server on the "host" side.

The hacker here is not breaking into the victim's computer to analyze the data being entered into the browser by the user. Only network traffic is analyzed to discover the desired information.

We observe here 2 scenarios, one where the victim is using a secure connection, through HTTPS (HyperText Transfer Protocol Secure) access, another where the victim uses a website with the standard HTTP (HyperText Transfer Protocol) protocol.

We will only analyze the first case where the victim is using a secure connection, after all, on the contrary, it would be trivial to observe the data being transacted in plain text.

Assuming that the victim is using an HTTPS connection, we can still see security issues. A web application consists of states and every time an action causes a state change, a connection is established with the server to obtain new data for the state in question. Now, even if the transacted data is encrypted, it is still possible to observe some attributes like packet size and time.

## Basics for accessing information

A web application is fundamentally similar to a traditional desktop application, both work in that the user enters input information into the application and a series of state transitions occurs until reaching the final result. The crucial difference between both models is that the web application that processing its states and its logic is divided into 2 steps, one on the client side (browser) and the other on the "host" side (server), so a subset of the program data stream is transacted over the internet. That piece of the data stream that transacts between the client and the server is vulnerable to attacks of this kind.

The attacker's objective is to infer information about the victim through the information he is able to extract from the encrypted packets they are transacting.

The hacker establishes an ambiguity reduction process where the ambiguity set of a dataset is given by all possible values that the data can assume. The effectiveness of the attack depends on the size of the ambiguity reduction that the attacker achieves and this serves as a measure for the leaked information. If the ambiguity set is reduced to 1/K of the original size, we say that the data set has lost log2K degrees of entropy.

## Attack model

A web application can be modeled as a tuple (S, σ, δ, f, V ), where:

1. S - Set of states that describe the application (example HTML DOM and cookies).

2. σ - Set of input that the application accepts from the user.

3. δ - Function that models the transition of states given by: δ : S × σ → S.

4. f - Function that models the observed data (such as packet size, number of packets...): f : S × σ → V , where V is the set of vectors that describe the characteristics of the observed encrypted data.

# How to infer sensitive data

Consider any given time t and any state st of the application that accepts input from the user. The user input dataset is then divided into k disjoint sets, each one taking the application to a different state reachable from st.

The hacker tries to discover the input data set that the application received in st from the observation of a sequence of vectors (vt, vt+1, ..., vt+n−1) generated by n consecutive changes of initiated states in st.

It is evident that the solution to this problem can be applied recursively from s0 to infer sensitive data from the states and transitions of a web application.

![Reduction of the ambiguity set](/assets/imgs/reductionOfTheAmbiguitySet.png){:class="img-fluid"}

The attacker is unaware of the input data in st before observing the sequence of vectors, all possible k sets of input data form the ambiguity set, which here has size k. By observing vt, the hacker knows that only transitions that lead to the subset St+1 denoted by Dt+1 can generate this vector in question, which ends up inferring that the input can only have come from b/α, where α ∈ [1, k), subsets of the input set is the transition state reduction factor. The new ambiguity set Dt+1 can be reduced again by looking at the other vectors (vt + 1, ..., vt+n−1).

## Practices to reduce susceptibility to attacks

As we have discussed previously, the reader must have already intuited that there is no way to completely protect against this type of attack, but there are good practices to improve the susceptibility rates to the attacks detailed here. The steps to be carried out during the development process are as follows:

1. I identify vulnerabilities The first step is to identify where the vulnerabilities are. As discussed in the previous chapter, each time the program executes a state transition that is associated with some user input, that transition is vulnerable.

There is no recipe here. It is up to the developer to perform an analysis of the application's data flow and user interaction to identify possible places where data is leaking.

2. Specify policies to mitigate "vulnerability" Here the user needs to analyze his application taking into account how web communication protocols, traffic patterns, data structure and transitions work.

# Attack between virtual machines

## Definition

With the migration of web applications from dedicated servers to "cloud" servers where the same physical machine is shared, an attack is made possible where one of the allocated virtual machines is
on one physical machine can infer the memory contents of a third virtual machine.

Here we need to pause to describe how virtual machine management works. A physical machine contains a virtual machine manager that runs and manages some virtual machines (we assume here 1 machine per core) where each virtual machine is in an isolated environment ("sandboxed") orchestrated by "hypervisor". To illustrate the problem in question and the attack vector I bring the quote: "In theory there is no difference between theory and practice. But in practice, there is".

Unsuspecting users are using software and libraries designed to run on unshared servers on shared servers. For example, an attack implementation by Bernstein uses cache hit time measurements to decrypt the AES key, where the attacker is able to retrieve the key with less than 100 user encrypting process.

## Cache Architecture

The cache consists of a small memory located between the CPU and RAM memory for quick access to frequently used data. It takes advantage of the cache's proximity to the CPU to avoid RAM memory access and, consequently, decrease the average memory access time. When a data is requested by the program, the processor first checks whether the data is present in the cache or not, if the data is, we say that there was a "cache hit", otherwise, we call it a "cache miss", when the " cache-miss", the processor looks for the data in main memory.

Is it already possible to identify where the cache architecture is essential to attack and discover the AES key? If a cache hit occurs during the encryption process, the time taken to execute the given instruction is shorter than when a cache miss occurs, as the processor will have to go to main memory and load the data block into the cache. Therefore, measuring the time to perform these operations it is possible to guess the AES key.

## The attack

A spy process is built that monitors the access to specific lines of the cache by the process being attacked. The attack consists of 3 states:

1. Flussing stage 

First the attacking process "flushes" the memory to ensure that when the attacked process needs data it is no longer in the cache and it has to access main memory. The hacker can implement this routine using, for example, the clflush command which not only cleans the cache of the core where the process is running but the cache of the entire physical machine and this is the crucial point of the attack, the attacker needs to ensure that he cleared the cache of all cores on the machine so that it is possible to measure the access time of another core to the cache and thus guess the AES cache of the "neighboring" virtual machine.

2. Target accessing stage

Here the attacker just waits for the user to execute a code fragment that is not in the cache and must be fetched from main memory.

3. Reloading stage

Here the attacker reloads the detected memory lines and measures the time the process takes. Depending on the time it takes to reload each line, the attacker decides which memory line is present in the cache (if the user has accessed that line in question) and which is not (if the user has not accessed this line). Here the hacker is taking advantage of the time difference between a "cache hit" and a "cache miss".

# Preventions

Here, unlike the web attack, we have a solution to solve the problem completely, using AES-NI instructions. In this case, the AES algorithm, instead of using data saved in memory to carry out the process of encrypting data, uses specific hardware instructions, thus preventing attacks that measure access to the cache.

Another method to mitigate this type of attack is to "pre-fetching" the T table used by AES in the encryption process before each round of algorithm execution. A hacker cannot measure the access time for each row in the table if the entire table is present in the cache before execution. A negative side of this implementation is that the tables are 4KB in size, which would occupy the entire cache, in addition to the fact that loading the entire table into the cache always takes considerable longer than the process where the entire table is not loaded into the cache. cache.

# Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)