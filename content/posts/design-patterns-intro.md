---
title: "Design patterns: what are they?"
date: 2019-08-02T10:30:00+08:00
draft: false
---
Software engineering is a messy process, but so is every creative human activity. Sometimes, I do wonder whether it is true that software engineering can be said to be a branch of "engineering". I leave that debate to another day, but in no uncertain terms, creating software is messy and often requires a lot of commitment, collaboration, and compromise from all stakeholders. In all my software engineering projects, I have not encountered a project that is smooth sailing from the onset and ends on a smooth sailing note.

To an extent however, software engineering is a formalised activity, though not necessarily theoretical. It is merely a body of knowledge built on experience and hard work. It is guided by best practices and rules of thumb, but never states any formal rule that may be proven mathematically (duh). These practices are tested in software projects, but always remain as inductive hypotheses, kind of like generalisations that may somehow fail in the future.

Thus, it is very important, at the onset, to recognise that the activity of software engineering is not always about *software* and neither is it always about *engineering*. Its rules will fail. These failures happen every time the app that you are using crashes, or some computer calculates something wrongly. We have to recognise that our applications - our creations - are fallible, but this does not hinder us nor discourage us from the pursuit of creating a bug-free application.

## Design Patterns

Beginners like myself often fall into poor design practices because we are trained in a certain manner and that is the only manner we will fall back onto when we confront problems. More experienced programmers have a wider range of vision and are able to see certain problems and group them. This comes, no doubt, with years of gruelling work programming many different applications and a certain proficiency in coding. But there is a way to "short-circuit" the learning process: design patterns.

As the Gang of Four writes, novelists and playwrights "rarely design their plots from scratch." Instead, they rely on stereotypes, archetypes, and templates from previous generations. Such an art "progresses" based on the efforts of previous generations to explore the uncertainties of writing and come up with novel (pun intended) ways of crafting their plot. Similarly, programmers can rely on the experience of past programmers to create and design according to fixed patterns.

The following series on software design patterns will document my journey in exploring such design patterns. I encountered some of them during my projects, but never actually formalised my understanding of them. This offers me the chance to understand it *for real* and with great seriousness. However, I must caveat and say that the design patterns in this series are for the object-oriented programming paradigm. There are many other kinds of design patterns which are equally, if not more, interesting that OOP ones, but OOP is the starting point for any beginner.

### Why use design patterns?

Design patterns allow us to create reproducible frameworks for a class of problems. Specific problems require specific approaches, which are expressed by design patterns. Every pattern "describes a problem which occurs over and over again in our environment, and then describes the core of the solution to that problem, in such a way that you can use this solution a million times over, without ever doing it the same way twice." A pattern has four essential elements:

1. The **pattern name** is the label that we give a specific design problem.
2. The **problem** is the situation/circumstance in which we apply the design pattern.
3. The **solution** contains all the elements that make up the design, relationships, responsibilities and collaborations.
4. The **consequences** are the pros and cons of the design pattern.

### How do we classify design patterns?

The original Gang of Four (GoF) book classifies design patterns into three main categories according to two axes: purpose (which shows what the pattern is supposed to do) and scope (which shows whether the pattern applies primarily to classes or to objects).

1. According to the purpose, we can classify patterns into 'creational', 'structural' or 'behavioural'.
2. According to the scope, we can classify patterns into 'applying to classes' or 'applying to objects'.

Recall that the design patterns described in the original GoF were meant for OOP. Finding the *appropriate problem* is often more important than the solution itself. Applying an inappropriate solution to a problem may cause more problems than intended. It is therefore very important and significant to understand the consequences of design patterns to prevent their incorrect usage.

## Object-Oriented Programming (OOP)

Here's a short refresher on OOP. The best way to think of OOP is using the following example:

> Tom kicks the ball.

OOP is a way of modelling the world around us. For example, `Tom` is a `Human` object and `the ball` is a `Ball` object. The difficulty of OOP is decomposing the world or situation into objects.

### Objects

When I first learnt OOP, we focused first on classes. However, this is fundamentally wrong. The nature of OOP - as its name suggests - is on *objects*, which represent the world. Classes, or interfaces for that matter, are really just templates of objects, and should be understood as such.

An object packages data and functions that act on the data. This is the act of *encapsulation*, which is fundamental to OOP programming. The functions that act on the data are known as *methods* or *operations*. An object performs an operation when it receives a message from a client. Thus, when the `Tom` object issues a `kick` operation, it is received by the `the ball` object.

Requests are literally the only way to get an object to execute an operation, and operations are the only way to change the object's internal data (which, by the way, can be made inaccessible or accessible based on *access modifiers*). The object's internal data is invisible from outside the object. The data is therefore said to be *encapsulated*.

#### Object interfaces lead to types

Every operation of an object has a name, its arguments/parameters, and the return value (it can be `void`). This is known as the operation's signature. The set of all *method signatures* is called the interface to the object. An object's interface characterises the complete set of requests that can be sent to the object.

Thus the concept of a *type* arises. A type is a particular interface, i.e., a unique set of certain method signatures. This leads us to an interesting inference:

1. An object can have many types. E.g., `Tom` can be a `Student`, `Human` and `Engineer` type.
2. Many objects can have the same type. E.g., `Prof Li` and `Tom` can be both an `Engineer` type.

Two objects of the same type need only share a part of their interface. Interfaces can contain other interfaces (through the `extends` keyword in Java, for example). Therefore,

> A type is a *subtype* if its interface contains the interface of its *supertype*.

The subtype therefore *inherits* the interface of its supertype, which is another fundamental point of OOP: inheritance (duh).

#### Why are interfaces so important?

Interfaces are the fundamental building block of OOP. Programmers are free to implement an interface however they want, but the interface **must** be there. The interface is akin to a "contract" that every class that implements the interface must specify.

If, for example, both the `Engineer` and `Student` class implement the `Human` interface, then they must both implement a method called `Work` in different ways. For example, an `Engineer` may do technical work, whereas a `Student`'s work may be writing an essay.

#### Abstract classes and mixin classes

An abstract class is a class whose main purpose is to define a common interface for its subclasses. An abstract class defers some, or all, of its implementation to methods defined in the subclasses. Hence, an abstract **cannot be instantiated**. The operations that an abstract class declares, but does not implement, are called abstract operations. Classes that are *not abstract* are called concrete classes.

A mixin class is a class that is intended to provide an optional interface, or functionality, to other classes. It is similar to an abstract class because it is not intended to be instantiated. Mixin classes require multiple inheritance.

#### Classes versus interfaces

An object's type is related to its interface (i.e., the unique set of methods). On the other hand, an object's class defines its implementation. Class inheritance and interface inheritance (or subtyping) are therefore two separate matters.

* Class inheritance defines an object's implementation in terms of another object's implementation.
* Interface inheritance describes when an object can be used in place of another.

In many languages, class and interface inheritance have the same language syntax for the sake of convenience. However, we should be conceptually clear about the difference.

#### Implementation in Java

In Java, which is a statically-typed OOP language, the following characteristics hold:

1. Interfaces define method signatures.
2. Abstract classes and concrete classes implement interfaces.
3. Classes may implement multiple interfaces but extend only one class.
4. Interfaces can inherit multiple interfaces.

Objects are created by instantiating a class. The object is said to be an instance of the class. The process of instantiating a class allocates storage for the objects internal data and associates the operations with these data.

### Best Practices

There are a couple of best practices that software engineering takes reference from. One of the most famous are the SOLID principles used to summarise what good software engineering is.

* Single responsibility principle: a class should have only one thing to do ("YOU HAD ONE JOB!").
* Open-closed principle: software entities should be open for extension but closed for modification.
* Liskov substitution principle: objects should be replaceable with instances of their subtypes without altering the correctness of that program.
* Interface segregation principle: many client-specific interfaces are better than one general-purpose interface.
* Dependency inversion principle: depend upon abstractions, not concretions.

In my opinion, the most important one (all are important, actually) is the single responsibility principle. The whole concept of software engineering is premised on the point that a component does only one thing and does it very well. It's the reason why UNIX has had such longevity. They have many little things that each do their own niche work really well. However, in terms of design, the underlying principle behind each of these principles is to *program to an interface, not an implementation*. This means that, as suggested by GoF, "commit only to an interface defined by an abstract class". When we abstract away creating objects, design patterns are possible.

#### Inheritance versus composition

Whether we use inheritance or composition to design our programs is a different matter altogether. Class inheritance allows us to define the implementation of one class in terms of another class, the superclass.

> Reusing by subclassing is known as white-box reuse.

It's called a "white-box" because we know the internals of the parent, which becomes visible after the subclassing. Unless, of course, someone decided to declare a certain variable as `private` rather than `public` or `protected` (which only allows subclasses to see).

On the other hand, "black-box" reuse is object composition. This is more of a bottom-up than top-down approach.

> Reusing by object composition is known as black-box reuse.

In black-box reuse, functionality is assembled together, or composed together by different objects. Object composition needs the objects being composed to have well-defined interfaces, although these objects themselves are "black-boxes" because we don't know what's going on inside them.

Try to favour composition over inheritance, because it makes sense. Inheritance implies an `is-a` relationship, which occurs much less often than a `has-a` relationship, which favours composition. If we go on to use inheritance in everything, what's going to happen is that we will have very large inheritance trees that duplicate each other's work.

#### Delegate, delegate, delegate

Delegation is not only good for projects (or... trying to avoid work?) but good for programming as well. Some call delegation a pattern, others think it is more fundamental and call it a principle. Whichever the case, it favours composition so I'll think it's important. Delegation "is a way of making composition as powerful for reuse as inheritance." Delegation involves two objects: a receiving object delegates operations to its delegate. In class inheritance, what happens is that the subclass will defer requests to the parent class, often using the `this` or `self` keyword (which refers to the *current instance*, i.e., the *current instantiated object*). For example:

```java
public class Human {
  public Human(int age) {
    this.age = age;
  }

  public void grow_old() {
    this.age += 10;
  }
}
```

However, with delegation, the receiver passes *itself* to the delegate to let the delegated operation refer to the receiver. This makes the `is-a` relationship distinct from a `has-a` relationship. For example, imagine we have a `Human` that has a `Liver`. Say the `Liver` is the only organ that can process alcohol.

```java
class Liver(float oxygen_amt) {
  public int process_alcohol() {
    // Process some alcohol...
    return this.alcohol_level;
  }
}

class Human(Liver liver) {
  // Delegate the calculation to the Liver
  public int process_alcohol() {
    return this.liver.process_alcohol();
  }
}
```
