---
layout:     post
title:      The Lambda in Java 8
categories: pattern
---

Now We know something about `Default Methods for Interfaces` from my last ![blog](http://zhiyuanma.github.io/pattern/2016/05/10/java-8-interface/).
Java 8 enables us to add non-abstract method implementations to interfaces by beginning with the `default` keyword. This feature is also known as Extension Methods. Here is our first example:

`
interface DoSomething {
    double cal(int a);

    default double sqrt(int a) {
        return Math.sqrt(a);
    }
}
`
Besides the abstract method `cal` the interface `DoSomething` also defines the default method `sqrt`. 
Sub-classes only have to implement the abstract method `cal`. The default method `sqrt` can be used out of the box.

`
DoSomething ds = new DoSomething() {
    @Override
    public double cal(int a) {
        return sqrt(a * 5);
    }
};

ds.calculate(5);     // 5.0
ds.sqrt(16);        // 4.0
`
