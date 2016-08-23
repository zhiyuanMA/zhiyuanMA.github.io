---
layout:     post
title:      The evolution of Interface in Java 8
categories: pattern
---

Designing interfaces have always been a tough job because if we want to add additional methods in the interfaces, it will require change in all the implementing classes. 

But the default interface methods and static interface methods in Java 8 accomplish to solve this pain.  

### Defualt Method
For creating a default method in java interface, we need to use “default” keyword with the method signature. For example,

![default 1 img](/images/interface/5_10_1.png)

Notice that log(String str) is the default method in the I1. Now when a class will implement I1, it is not mandatory to provide implementation for default methods of interface. This feature will help us in extending interfaces with additional methods, all we need is to provide a default implementation.


Let’s say we have another interface with following methods:

![default 2 img](/images/interface/5_10_2.png)

We know that Java doesn’t allow us to extend multiple classes because it will result in the “Diamond Problem” where compiler can’t decide which superclass method to use. With the default methods, the diamond problem would arise for interfaces too. Because if a class is implementing both I1 and I2 and doesn’t implement the common default method, compiler can’t decide which one to chose.

![default err img](/images/interface/5_10_3.png)

So to make sure, this problem won’t occur in interfaces, it’s made mandatory to provide implementation for common default methods of interfaces. So if a class is implementing both the above interfaces, it will have to provide implementation for log() method otherwise compiler will throw compile time error. But we also could use the I1's default method in the following example,

![default err img](/images/interface/5_10_4.png)

#### Tips of Default Method

1.  Java interface default methods will help us in extending interfaces without having the fear of breaking implementation classes.
2.  Java interface default methods has bridge down the differences between interfaces and abstract classes.
3.  Java 8 interface default methods will help us in avoiding utility classes, such as all the Collections class method can be provided in the interfaces itself.
4.  If any class in the hierarchy has a method with same signature, then default methods become irrelevant. A default method cannot override a method from `java.lang.Object`. The reasoning is very simple, it’s because `Object` is the base class for all the java classes. So even if we have `Object` class methods defined as default methods in interfaces, it will be useless because Object class method will always be used. That’s why to avoid confusion, we can’t have default methods that are overriding Object class methods. 
5.  There could be many default methods in an interface


### Static Method
Java interface static method is similar to default method, but we cannot override them in the implementation classes(there could be a method with same name). This feature helps us in avoiding undesired results in case of poor implementation in sub classes. Let’s look into this with a simple example,

![default err img](/images/interface/5_10_5.png)

Notice that *Java interface static method is visible to interface methods only*, the sub classes cannot use the static interface methods. However like other static methods, we can use interface static methods using interface name.

#### Tips of Static Method
1.  Java interface static method is part of interface, we cannot use it for implementation class objects.
2.  Java interface static methods are good for providing utility methods, for example null check, collection sorting etc.
3.  Java interface static method helps us in providing security by not allowing implementation classes to override them.
4.  We cannot define interface static method for `Object` class methods, we will get compiler error as “This static method cannot hide the instance method from Object”. This is because it’s not allowed in java, since `Object` is the base class for all the classes and we cannot have one class level static method and another instance method with same signature.
5.  An interface could have both many default methods and static methods at the same time.


### Functional Interface
One of the major reason for introducing default methods in interfaces is to enhance the Collections API in Java 8 to support **lambda expressions**.


An interface with exactly one abstract method is known as Functional Interface.A new annotation @FunctionalInterface has been introduced to mark an interface as Functional Interface. @FunctionalInterface annotation is a facility to avoid accidental addition of abstract methods in the functional interfaces. It’s optional but good practice to use it.

![default f img](/images/interface/5_10_6.png)


@Functional interfaces are long awaited and much sought out feature of Java 8 because it enables us to use lambda expressions to instantiate them. A new package java.util.function with bunch of functional interfaces are added to provide target types for lambda expressions and method references. 

I will look into functional interfaces and lambda expressions in the future posts.

