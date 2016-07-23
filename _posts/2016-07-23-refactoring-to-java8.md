---
layout: post
title:  "Refactoring Code - Applying Java 8 Features"
date:   2016-07-23
excerpt: "Demonstrates some of the Java 8 features we can use to implement common Java 8 refactorings."
tag:
- java 8
- lambda
- streams
- refactoring
- migration
comments: true
---

## Overview

As we draw closer to Java 9, Java 8 is starting to reach wide usage and many of us are still looking to implement some of
the common Java 8 features into our existing code base. In this post, I share of the refactorings I have applied so far
in my experience.

## Why Refactor to Java 8?

* Performance Improvements in some of the common Data Structures
* Enables Parallelization
* Fewer Lines of Code

## Anonymous classes can be replaced with Lambda Expressions

**Functional Interfaces** and **Lambda Expressions** will make your code more concise. Lambda is an implementation of a
Functional Interface. Lambda expressions are simply anonymous methods which can replace the many lines of codes from
an anonymous inner class.

Consider the *Runnable* interface:

{% highlight java %}
@FunctionalInterface
public interface Runnable {
    public abstract void run();
}
{% endhighlight %}

You may come across a `Runnable` anonymous inner class:

{% highlight java %}
    executorService.submit(new Runnable() {
            @Override
            public void run() {
                System.out.println("Hello from " + Thread.currentThread().getName());
            }
        });
{% endhighlight %}

We can shrink the above down to a Lambda:

{% highlight java %}
executorService.submit(() -> System.out.println("Hello from " + Thread.currentThread().getName()));
{% endhighlight %}

So, instead of a full *Runnable*, we have a small *Lambda* with fewer lines of code, reducing boilerplate and better
readability.