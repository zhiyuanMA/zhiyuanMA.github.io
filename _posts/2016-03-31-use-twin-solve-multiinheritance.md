---
layout:     post
title:      Use the TWIN to solve multi-inheritance in Java
categories: pattern
---

Multiple inheritance allows one to inherit data and code from more than one base class. It is a controversial feature that
is claimed to be indispensable by some programmers, but also blamed for problems by others, since it can lead to name clashes, complexity and inefficiency. In most cases, software architectures become cleaner and simpler when multiple inheritance is avoided, but there are also situations where this feature is really needed. If one is programming in a language that does not support multiple inheritance (e.g. in Java), but if one really needs this feature, one has to find a work-around. The Twin pattern — introduced in this paper—provides a standard solution for such cases. It gives one most of the benefits of multiple inheritance while avoiding many of its problems.


As a motivating example for a situation that requires multiple inheritance, consider a computer ball game consisting of active and passive game objects. The active objects are balls that move across the screen at a certain speed. The passive objects are paddles, walls and other obstacles that are either fixed at a certain screen position or can be movedunder the control of the user.


The body of a ball thread is implemented in its run() method. When a ball thread is running, it repeatedly moves and draws the ball. If the user clicks on a ball, the ball sends itself a suspend() message to make the ball moving again.


The important thing about this example is that balls are both game items and threads. They can be linked into a list of game items, for example, so that they can be sent draw() and intersects() messages. But they can also be linked into a list of threads from which the scheduler selects the next thread to run. Thus, ball have to be compatible with both base classes. This is a typical case where multi-inheritance is useful.

![game item img](/images/twin/1.png)

### Applicability
