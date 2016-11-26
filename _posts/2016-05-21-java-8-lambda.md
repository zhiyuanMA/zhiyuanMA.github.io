---
layout:     post
title:      The Lambda in Java 8
categories: Java
---

<link rel="stylesheet" type="text/css" href="/js/prettify/css/github.css" />
Now We know something about `Default Methods for Interfaces` from my last [blog](http://zhiyuanma.github.io/pattern/2016/05/10/java-8-interface/).
Java 8 enables us to add non-abstract method implementations to interfaces by beginning with the `default` keyword. This feature is also known as Extension Methods. Here is our first example:

```java
interface DoSomething {
    double cal(int a);

    default double sqrt(int a) {
        return Math.sqrt(a);
    }
}
```

Besides the abstract method `cal` the interface `DoSomething` also defines the default method `sqrt`. 
Sub-classes only have to implement the abstract method `cal`. The default method `sqrt` can be used out of the box.

```java
DoSomething ds = new DoSomething() {
    @Override
    public double cal(int a) {
        return sqrt(a * 5);
    }
};

ds.calculate(5);     // 5.0
ds.sqrt(16);        // 4.0
```

The `ds` is implemented as an anonymous object. The code is quite verbose: 6 lines of code for such a simple method. 
In the next section, there's a much nicer way of implementing single method objects in Java 8.

## Lambda expressions
How to sort a list of strings in prior versions of Java, there is an example:

```java
List<String> names = Arrays.asList("a", "b", "c", "d");
Collections.sort(names, new Comparator<String>() {
    @Override
    public int compare(String a, String b) {
        return b.compareTo(a);
    }
});
```

The static utility method `Collections.sort` accepts a list and a comparator in order to sort the elements of the given list. 
You often find yourself creating anonymous comparators and pass them to the `sort` method.
Instead of creating anonymous objects all day long, Java 8 comes with a much shorter syntax, lambda expressions:

```java
Collections.sort(names, (String a, String b) -> {
    return b.compareTo(a);
});
```

The code is much shorter and easier to read. But it gets even shorter:

```java
Collections.sort(names, (String a, String b) -> b.compareTo(a));
```

For one line method bodies you could skip both the braces {} and the `return` keyword. Now it gets even more shorter:

```java
Collections.sort(names, (a, b) -> b.compareTo(a));
```

The java compiler is aware of the parameter types so you can skip them as well. 
Let's dive deeper into how lambda expressions can be used in the wild. I'll begin with the `Functional Interface`.


Each lambda corresponds to a given type, specified by an interface -- functional interface must contain exactly one abstract method declaration. 
Each lambda expression of that type will be matched to this abstract method. Since default methods are not abstract you're free to add default methods to your functional interface.

We can use arbitrary interfaces as lambda expressions as long as the interface only contains one abstract method. 
To ensure that your interface meet the requirements, you could add the `@FunctionalInterface` annotation. 
The compiler is aware of this annotation and throws a compiler error as soon as you try to add a second abstract method declaration to the interface.

```java
@FunctionalInterface
interface Transformer<F, T> {
    T transform(F before);
}
Transformer<String, Integer> transformer = (before) -> Integer.valueOf(before);
Integer after = transformer.transform("123");
System.out.println(after);    // 123
```

Keep in mind that the code is also valid if you forget the `@FunctionalInterface` annotation.

## Method and Constructor References
The above example code can be further simplified by utilizing static method references:

```java
Transformer<String, Integer> transformer = Integer::valueOf;
Integer after = transformer.transform("123");
System.out.println(after);   // 123
```

Java 8 enables you to pass references of methods or constructors via the :: keyword. 
The above example shows how to reference a static method. But we can also reference object methods:

```java
class Something {
    String startsWith(String s) {
        return String.valueOf(s.charAt(0));
    }
}
Something something = new Something();
Transformer<String, String> transformer = something::startsWith;
String after = transformer.transform("Java");
System.out.println(after);    // "J"
```

There are three main kinds of method references:
*  A method reference to a static method (for example, the method parseInt of Integer, written Integer::parseInt)
*  A method reference to an instance method of an arbitrary type (for example, the method length of a String, written `String::length`)
*  A method reference to an instance method of an existing object (for example, suppose you have a local variable `transformer` 
that holds an object of type `Transformer`, which supports an instance method `getValue`; you can write `transformer::getValue`)


Let's see how the `::` keyword works for constructors. First we define an example bean with default constructors:

```java
class Person {
    String name;

    Person() {}
}

Supplier<Person> sp1 = Person::new;
```

This is equivalent to 


```java
Supplier<Person> sp2 = ()->new Person();
```

What will happen if you have a constructor with signature, it fits the signature of the Function interface, so you can do this,

```java
class Person {
    String name;

    Person() {}

    Person(String name){
        this.name = name;
    }
}

Function<String,Person> fp = s->new Person(s);
fp.apply("Mark");

Function<String,Person> fp1 = Person::new;
fp1.apply("Mark");
```

But if `Person` has a constructor with 3 or more signatures, what should we do? Next we specify a person factory interface to be used for creating new persons:

```java
interface PersonFactory<P extends Person> {
    P create(String firstName, String middleName, String lastName, int age, double weight);
}
```

Instead of implementing the factory manually, we glue everything together via constructor references:

```java
PersonFactory<Person> personFactory = Person::new;
Person person = personFactory.create("Mark", "" , "Ma", 27, 65);
```

We create a reference to the Person constructor via `Person::new`. The Java compiler automatically chooses the right constructor by matching the signature of `PersonFactory.create`.


## Lambda Scopes
Accessing outer scope variables from lambda expressions is very similar to anonymous objects. 
Lambda can access final variables from the local outer scope as well as instance fields and static variables.

#### Accessing local variables

read final local variables from the outer scope of lambda expressions:

```java
final int num = 1;
Transformer<Integer, String> stringTranformer =
        (from) -> String.valueOf(from + num);

stringTranformer.convert(2);     // 3
```

But different to anonymous objects the variable num does not have to be declared `final`. This code is also valid:

```java
int num = 1;
Transformer<Integer, String> stringTranformer =
        (from) -> String.valueOf(from + num);

stringTranformer.convert(2);     // 3
```

However num must be implicitly final for the code to compile. The following code does not compile:

```java
int num = 1;
Transformer<Integer, String> stringTranformer =
        (from) -> String.valueOf(from + num);
num = 3;
```

Writing to num from within the lambda expression is also prohibited.

#### Accessing fields and static variables

In constrast to local variables we have both read and write access to instance fields and static variables in lambda expressions. 
This behaviour is well known from anonymous objects.

```java
class Lambda4 {
    static int outerStaticNum;
    int outerNum;

    void testScopes() {
        Tranformer<Integer, String> stringTranformer1 = (before) -> {
            outerNum = 2;
            return String.valueOf(before);
        };

        Tranformer<Integer, String> stringTranformer2 = (before) -> {
            outerStaticNum = 7;
            return String.valueOf(before);
        };
    }
}
```

#### Accessing Default Interface Methods

Remember the `DoSomething` example from the first section? `DoSomething` defines a default method `sqrt` which can be accessed from each instance including anonymous objects. 
This does not work with lambda expressions.
Default methods cannot be accessed from lambda expressions. The following code does not compile:

```java
Formula formula = (a) -> sqrt( a * 100);
```

## Built-in Functional Interfaces

The JDK 1.8 API contains many built-in functional interfaces. 
Some of them are well known from older versions of Java like `Comparator` or `Runnable`. 
Those existing interfaces are extended to enable Lambda support via the `@FunctionalInterface` annotation.

But the Java 8 API is also full of new functional interfaces to make your work easier. 
Some of those new interfaces are well known from the `Google Guava` library. 

#### Predicates

`Predicates` are boolean-valued functions of one argument. The interface contains various default methods for composing predicates to complex logical terms (and, or, negate)

```java
Predicate<String> predicate = (s) -> s.length() > 0;

predicate.test("foo");              // true
predicate.negate().test("foo");     // false

Predicate<Boolean> nonNull = Objects::nonNull;
Predicate<Boolean> isNull = Objects::isNull;

Predicate<String> isEmpty = String::isEmpty;
Predicate<String> isNotEmpty = isEmpty.negate();
```

#### Functions

`Functions` accept one argument and produce a result. Default methods can be used to chain multiple functions together (compose, andThen).

```java
Function<String, Integer> toInteger = Integer::valueOf;
Function<String, String> backToString = toInteger.andThen(String::valueOf);

backToString.apply("123");     // "123"
```

#### Suppliers

`Suppliers` produce a result of a given generic type. Unlike `Functions`, `Suppliers` don't accept arguments.

```java
Supplier<Person> personSupplier = Person::new;
personSupplier.get();   // new Person
```

#### Consumers

`Consumers` represents operations to be performed on a single input argument.

```java
Consumer<Person> greeter = (p) -> System.out.println("Hello, " + p.firstName);
greeter.accept(new Person("Marm", "Ma"));
```

#### Comparators

`Comparators` are well known from older versions of Java. Java 8 adds various default methods to the interface.

```java
Comparator<Person> comparator = (p1, p2) -> p1.firstName.compareTo(p2.firstName);

Person p1 = new Person("Mark", "Ma");
Person p2 = new Person("Donald", "Trump");

comparator.compare(p1, p2);             // > 0
comparator.reversed().compare(p1, p2);  // < 0
```