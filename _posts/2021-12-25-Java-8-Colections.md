---
layout: posts
title: Java 8 collections
icon: fa-comment-alt
tag: java tutorial
categories: javs
---

Java 8 Collections refers to the various classes and interfaces available in the java.util package that enable the storage and manipulation of data in a program. It includes List, Set, Map, and Queue interfaces, as well as their various implementations such as ArrayList, HashSet, HashMap, and LinkedList.

# Sections

1. [Java 8 collections](#java-8-collections)

2. [Conclusion](#conclusion)

## Java 8 collections

List Interface: It is an ordered collection of elements and can contain duplicates. The ArrayList class implements the List interface and provides an array-based implementation.

```java
List<Integer> numbers = new ArrayList<>();
numbers.add(1);
numbers.add(2);
numbers.add(3);
numbers.add(1);
````

Set Interface: It is an unordered collection of elements and does not allow duplicates. The HashSet class implements the Set interface and provides a hash-table based implementation.

```java
Set<String> words = new HashSet<>();
words.add("dog");
words.add("cat");
words.add("bird");
words.add("dog"); // This will not be added as set does not allow duplicates.
````

Map Interface: It is a collection of key-value pairs, where each key is unique. The HashMap class implements the Map interface and provides a hash-table based implementation.

```java
Map<String, Integer> grades = new HashMap<>();
grades.put("John", 85);
grades.put("Jane", 90);
grades.put("Jim", 80);
````

Queue Interface: It is an interface that represents a collection of elements that are waiting to be processed and can be accessed in a specific order. The LinkedList class implements both the List and Queue interfaces and provides a linked-list based implementation.

```java
Queue<String> tasks = new LinkedList<>();
tasks.offer("Task 1");
tasks.offer("Task 2");
tasks.offer("Task 3");

````

Stream API: Java 8 introduced a new Stream API that provides functional-style operations on collections. It allows for the processing of data in a parallel and functional manner.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
numbers.stream().filter(n -> n % 2 == 0).forEach(System.out::println);
````

Lambda Expressions: Java 8 also introduced Lambda Expressions, which are anonymous functions that can be treated as values. They are used to simplify the code and make it more readable.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
numbers.forEach(n -> System.out.println(n));
````

Method References: Method references are a way to refer to a method by name. They can be used as a shorthand for a lambda expression that simply calls an existing method.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
numbers.forEach(System.out::println);
````

Default Methods: Java 8 also introduced default methods, which are methods that have a default implementation in the interface. This allows for the addition of new functionality to existing interfaces without breaking existing implementations.

## Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)