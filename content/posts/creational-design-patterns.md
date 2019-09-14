---
title: "Creational design patterns"
date: 2019-09-07T01:09:32+08:00
draft: false
---

As was written in the previous [post]({{< relref "design-patterns-intro.md" >}}), design patterns can be categorised according to purpose (creational, structural, and behavioural) and scope (applying to classes or applying to objects). Today we'll focus on creational design patterns, which have to do with *instantiation* and abstracting away that instantiation process. What is instantiation, though? It is the act of creating an object from a given class. Creational design patterns are then further classified according to their scope:

* A class creational pattern will use inheritance to vary the class that is instantiated.
* An object creational pattern will delegate instantiation to another object (sounds familiar? This the delegation principle that was covered previously as well).

Creational patterns give a lot of flexibility in choosing what gets created, who creates it, how it is created, and when it is created. It allows you to specify the instantiation of objects granularly. The common ones are:

* Prototype
* Abstract factory
* Builder
* Singleton

We will look at each one of these in turn.

## Factory and Abstract Factory

The factory and abstract factory patterns are both
