---
layout:     post
title:      The Lambda in Java 8
categories: pattern
---

<link rel="stylesheet" type="text/css" href="/js/prettify/css/github.css" />
Now We know something about `Default Methods for Interfaces` from my last [blog](http://zhiyuanma.github.io/pattern/2016/05/10/java-8-interface/).
Java 8 enables us to add non-abstract method implementations to interfaces by beginning with the `default` keyword. This feature is also known as Extension Methods. Here is our first example:

<pre class="prettyprint">
interface DoSomething {
    double cal(int a);

    default double sqrt(int a) {
        return Math.sqrt(a);
    }
}
</pre>

Besides the abstract method `cal` the interface `DoSomething` also defines the default method `sqrt`. 
Sub-classes only have to implement the abstract method `cal`. The default method `sqrt` can be used out of the box.

<pre class="prettyprint">
DoSomething ds = new DoSomething() {
    @Override
    public double cal(int a) {
        return sqrt(a * 5);
    }
};

ds.calculate(5);     // 5.0
ds.sqrt(16);        // 4.0
</pre>

The `ds` is implemented as an anonymous object. The code is quite verbose: 6 lines of code for such a simple method. 
In the next section, there's a much nicer way of implementing single method objects in Java 8.

## Lambda expressions
How to sort a list of strings in prior versions of Java, there is an example:

<pre class="prettyprint">
List<String> names = Arrays.asList("a", "b", "c", "d");
Collections.sort(names, new Comparator<String>() {
    @Override
    public int compare(String a, String b) {
        return b.compareTo(a);
    }
});
</pre>

The static utility method `Collections.sort` accepts a list and a comparator in order to sort the elements of the given list. You often find yourself creating anonymous comparators and pass them to the `sort` method.
Instead of creating anonymous objects all day long, Java 8 comes with a much shorter syntax, lambda expressions:

<pre class="prettyprint">
Collections.sort(names, (String a, String b) -> {
    return b.compareTo(a);
});
</pre>

The code is much shorter and easier to read. But it gets even shorter:

<pre class="prettyprint">
Collections.sort(names, (String a, String b) -> b.compareTo(a));
</pre>

For one line method bodies you could skip both the braces {} and the `return` keyword. Now it gets even more shorter:

<pre class="prettyprint">
Collections.sort(names, (a, b) -> b.compareTo(a));
</pre>

The java compiler is aware of the parameter types so you can skip them as well. 
Let's dive deeper into how lambda expressions can be used in the wild. I'll begin with the `Functional Interface`.
Each lambda corresponds to a given type, specified by an interface -- functional interface must contain exactly one abstract method declaration. 
Each lambda expression of that type will be matched to this abstract method. Since default methods are not abstract you're free to add default methods to your functional interface.

We can use arbitrary interfaces as lambda expressions as long as the interface only contains one abstract method. 
To ensure that your interface meet the requirements, you could add the `@FunctionalInterface` annotation. 
The compiler is aware of this annotation and throws a compiler error as soon as you try to add a second abstract method declaration to the interface.

<pre class="prettyprint">
@FunctionalInterface
interface Transformer<F, T> {
    T transform(F before);
}
Transformer<String, Integer> transformer = (before) -> Integer.valueOf(before);
Integer after = transformer.transform("123");
System.out.println(after);    // 123
</pre>

Keep in mind that the code is also valid if you forget the `@FunctionalInterface` annotation.

The above example code can be further simplified by utilizing static method references:


## Method and Constructor References
The above example code can be further simplified by utilizing static method references:

<pre class="prettyprint">
Transformer<String, Integer> transformer = Integer::valueOf;
Integer after = transformer.transformer("123");
System.out.println(after);   // 123
</pre>