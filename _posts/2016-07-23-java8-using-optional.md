---
layout: post
title:  "Using Optionals"
date:   2016-08-01
excerpt: "Discusses what Optionals are in Java 8 and illustrates some examples."
tag:
- java 8
- optional
- refactoring
- migration
- NullPointerException
comments: true
---

## 1. What are Optionals in Java 8?

`java.util.Optional` gives us a way to handle null values, to prevent `NullPointerException`. They are a simple container
 for a value which may be null or non-null. *Java 8 Optional<T>* has been added to deal with optional object references.
 The intention here is not to replace every null-able references, but to simplify the code and improve readability.

 The idea is, it works like a container for the type object *T*. A value of this object is returned if this value is not a
 *null* value. If we get a *null* value in this container, then it will allows us to do some predefined actions instead
 of throwing `NullPointerException`.

## 2. How do we create Optional<T> instances?

Create an instance of *Optional* class using its static method:

{% highlight java %}
Optional<String> optional = Optional.empty();
{% endhighlight %}

Return an *Optional* which contains not-null value:

{% highlight java %}
String str = "string value";
Optional<String> optional = Optional.of(str);
{% endhighlight %}

Return an *Optional* with the specified value, if non-null, otherwise return an empty *Optional*:

{% highlight java %}
Optional<String> optional = Optional.ofNullable(getString());
{% endhighlight %}

## 3. Convenient Methods in Optional Class

* `isPresent()`: return true if a value is present in **Optional**.
* `get()`: if a value is present, returns a reference to the item contained in the optional object. Otherwise throws a *NoSuchElementException*.
* `orElse(T other)`: returns the value, if present, otherwise return the value held in other.
* `ifPresent(Consumer<? super T> consumer)`: If a value is present, invoke the specified consumer with the value, otherwise do nothing.

{% highlight java %}

        Optional<String> optional = Optional.of("hello");

        optional.isPresent(); // true
        optional.get(); // "hello"
        optional.orElse("no"); // "hello"

        optional.ifPresent(s -> System.out.println(s.charAt(0)));

{% endhighlight %}

The above are some of the common methods I've used. There are other convenient methods that the **Optional** class supports.


## 4. Avoiding Null Checks example

{% highlight java %}

public class OptionalExample2 {

    public static void main(String[] args) {
        List<Trade> tradeList = new ArrayList<>();
        tradeList.add(new Trade("John", 100, Trade.Side.BUY));
        tradeList.add(new Trade("Mary", 200, Trade.Side.BUY));
        tradeList.add(new Trade("Andy", 50, Trade.Side.SELL));

        Optional<Trade> foundTrades = find("Sam", tradeList);
        System.out.println(foundTrades.orElse(new Trade("Sam", 60, Trade.Side.SELL)));

    }

    private static Optional<Trade> find(String tradername, List<Trade> tradeList) {
        for (Trade trade : tradeList) {
            if (trade.getTraderName().equals(tradername)) {
                return Optional.of(trade);
            }
        }
        return Optional.empty();
    }
}

{% endhighlight %}

The *find()* method signature here tells you that the method return a trade optionally. In this example, I have made the
`foundTrades` field into an `Optional` of `Trade`, so that I can use the methods on `Optional` instead of performing
null-checks. Call to `orElse(T other)` will perform the null-checks on the value and return other if the value is not
present.


## 5. Conclusions

Null checks are quite cumbersome. But with Java 8, we can use `Optional` class techniques to prevent writing needless
 null checks using new features like lambda expressions.

 For a further understanding and examples of use of Optionals, take a look at the following article:
 [Avoiding Null Checks with Java 8](http://winterbe.com/posts/2015/03/15/avoid-null-checks-in-java/)

 If you are looking at Optionals from a Migration perspective, have a read at JetBrain's guide on using Optionals:
  [Using Optionals](https://www.jetbrains.com/help/idea/2016.1/tutorial-migrating-to-java-8.html#d355044e686)


The source code for accompanying this article can be found on [GitHub](https://github.com/mahadyhasan/tutorials/tree/master/core-java8)

