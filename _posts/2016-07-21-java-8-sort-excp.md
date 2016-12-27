---
layout:     post
title:      An Exception in Java 8 
categories: Java
---

Java 8 is amazing! So I transfer all my works to new version of Java, and persuade my workmates to do the same work. 

`But we received an exception when ran the test code, OMG`

The exception said:

```java
System.err: java.lang.IllegalArgumentException: Comparison method violates its general contract!
System.err: at java.util.TimSort.mergeLo(TimSort.java:743)
System.err: at java.util.TimSort.mergeAt(TimSort.java:479)
System.err: at java.util.TimSort.mergeCollapse(TimSort.java:406)
System.err: at java.util.TimSort.sort(TimSort.java:210)
System.err: at java.util.TimSort.sort(TimSort.java:169)
```

my code is simple, it's just a **Stream.sorted** method with lambda-expression statement:

```java
stream().sorted((o1, o2) -> o1 > o2 ? 1 : -1).limit(count).forEach(s -> result.add(s.kw + "," + key + "," + s.rate));
```

The problem is the Sorted method, which changed the core algorithm after Java 7.

We can see the words in the Java docs:

`
Compares its two arguments for order. Returns a negative integer, zero, or a positive integer as the first argument is less than, equal to, or greater than the second.

In the foregoing description, the notation sgn(expression) designates the mathematical signum function, which is defined to return one of -1, 0, or 1 according to whether the value of expression is negative, zero or positive.

The implementor must ensure that sgn(compare(x, y)) == -sgn(compare(y, x)) for all x and y. (This implies that compare(x, y) must throw an exception if and only if compare(y, x) throws an exception.)

The implementor must also ensure that the relation is transitive: ((compare(x, y)>0) && (compare(y, z)>0)) implies compare(x, z)>0.

Finally, the implementor must ensure that compare(x, y)==0 implies that sgn(compare(x, z))==sgn(compare(y, z)) for all z.

It is generally the case, but not strictly required that (compare(x, y)==0) == (x.equals(y)). Generally speaking, any comparator that violates this condition should clearly indicate this fact. The recommended language is "Note: this comparator imposes orderings that are inconsistent with equals." 
`

The most important thing this doc tells us is we have to make sure **sgn(compare(x, y)) == -sgn(compare(y, x))**, so the code should like the below:

```java
if (o1.rate - o2.rate == 0) {
    return 0;
} else if (o1.rate - o2.rate > 0) {
    return -1;
} else {
    return 1;
}
```

That means we should make sure `compare(x,y)` must return zero when x==y.


**OR**


use those expression:

```java
//in command line
java -Djava.util.Arrays.useLegacyMergeSort=true  

//in code
System.setProperty("java.util.Arrays.useLegacyMergeSort","true");
```

It's really not an easy way to upgrade the Java version, thanks to our Test Codes!
