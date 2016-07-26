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

## 1. Overview

As we draw closer to Java 9, Java 8 is starting to reach wide usage and many of us are still looking to implement some of
the common Java 8 features into our existing code base. In this post, I share some of the refactorings I have applied so far
in my experience.

## 2. Why Refactor to Java 8?

* Performance Improvements in some of the common Data Structures
* Enables Parallelization
* Fewer Lines of Code

## 3. Lambda Expressions

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
readability. This *lambda expression* style can be used for any suitable interface from old java libraries. We have seen
one above with *Runnable*, but take a look at *Comparator* and so on online.

## 4. From Collection to Streams

In real-world applications, it is quite common to process collections in a number of stages. A collection type is usually
iteratively processed to produce another new collection, which then in turn is iteratively processed, and so on. Consider
this example:

{% highlight java %}
        //start with a collection of Integer instances
        List<Integer> intList = Arrays.asList(1, 2, 3, 4, 5);

        List<Point> pointList = new ArrayList<>();

        //transformation to produce a collection of Point Instances
        for (Integer i : intList) {
            pointList.add(new Point(i % 3, i / 1));
        }

        //find the maximum among the distances of each Point
        double maxDistance = Double.MIN_VALUE;
        for (Point p : pointList) {
            maxDistance = Math.max(p.distance(0, 0), maxDistance);
        }
{% endhighlight %}

We see many examples of code of this pattern today. If we look closely, some of the unpleasant things that stand out are:

* Verbose - 9 lines of code for three operations
* `pointList` is intermediate storage - it is an overhead on the program

**Collections** can now be exposed as a **stream** from Java 8. *Streams* are sequence of consumable elements for the consumption of an operation or iteration.
They are different from collections in that they don't store values; their purpose is to process them. We can use streams to refactor the above code.

With this type of refactoring, we can take anything that looks like a for loop and we can turn that into a bunch of
different things and our IDE or us as developers can decide what is the correct pattern for the piece of code.

So, compared to the successive iterations in the code above, we can provide a concise code with the use of streams as follows:

{% highlight java %}
       //Java 8 Refactor
        OptionalDouble maxDistance =
                intList.stream()
                        .map(i -> new Point(i % 3, i / 3))
                        .mapToDouble(p -> p.distance(0, 0))
                        .max();
{% endhighlight %}

And guess what? We have successfully removed the intermediate collection with the refactoring and therefore reduced the
overhead that comes with creating a list. This piece of code is also double the faster as the loop version.

## 5. Conclusions

In this article I briefly touched upon refactoring existing Java code to Java 8 code making use of Lambdas and Streams.
The goal of this writeup was only to provide a very quick introduction to how we can refactor to Java 8 code.

There are many more examples of refactorings that can be applied to your code and most modern IDEs can step in here and help
with inspections. If you are using IntelliJ IDEA for your java development, they have written a very useful article
about [Migrating to Java 8](https://www.jetbrains.com/help/idea/2016.1/tutorial-migrating-to-java-8.html#d355044e36)
which I highly recommended you to read.

The source code for accompanying the article can be found on [GitHub](https://github.com/mahadyhasan/tutorials/tree/master/core-java8)

