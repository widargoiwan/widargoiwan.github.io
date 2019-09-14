---
title: "HCI Concepts"
date: 2019-09-15T02:19:57+08:00
draft: true
---

Human-computer interaction (HCI) involves designing the user interface (UI) and the user experience (UX). This post will focus on the **design principles** of used to craft elegant interfaces and sensible experiences.

## Cognition

We have to consider cognition first. There are many different types of cognition:

* Experimental cognition is a state of mind when people perceive, act, and react to events around them intuitively and effortlessly.
* Reflective cognition involves mental effort, attention, judgement, and decision-making.

If you have read Kahneman (2011), experimental cognition corresponds to the Type I system and reflective cognition corresponds to the Type II system. Esyenck and Brysbaert (2018) describes cognition in terms of the following processes: attention, perception, memory, learning, language (reading, speaking, listening), and executive functions (problem-solving, planning, reasoning, and decision-making).

* The extent to which *attention* maintained depends on: (1) the user having clear goals and (2) presentation of information (the information-searching tasks must be precise, and require specific answers). Very often, people "multi-task". However, it is very much like how a single-core computer multi-tasks by switching between two processes. Heavy multi-taskers are actually the most distracted people, because they are switching between tasks at a very high frequency, with a lot of cognitive overload.
* Perception is how information is acquired from the environment.
* Memory involves recalling various kinds of knowledge that allow people to act appropriately. For example, people are much better at *recognising* things rather than *recalling* things. A number of methods take advantage of this, by using meta-data and cookies to store a user's preferences, and then providing a tailored search history (e.g., Google). Others, such as PayPal, offload the cognitive burden of remembering one's credit card details.
* Learning is closely associated with memory, and involves the accumulation of skills and knowledge that would be impossible to achieve without memory. There are two types of learning: incidental and intentional learning.
* Language
* Executive functions

### Mental Models

Mental models are deployed by users to understand how a technology works. People can deploy the incorrect mental model, causing failure to use the technology properly. UX designers can help people to develop better mental models by providing:

* Clear and easy-to-follow instructions.
* Appropriate documentation and tutorials.
* Background information.
* Affordances.

#### Affordances

Affordance is a fundamental concept in HCI. In a nutshell, an affordance is an artefact or cue that provides the user with a clue as to how the technology should be used. The Interaction Design Foundation gives a good overview of affordance [here](https://www.interaction-design.org/literature/topics/affordances).

According to Norman (1988), affordances are perceivable action possibilities: actions that users consider possible. An object's affordances thus depends on the users' physical capabilities and their goals and past experiences.

## Design Frameworks

There are three important frameworks that can be used to construct design principles:

1. Norman's Action Model
2. Norman's Gulf Theory
3. Schneiderman's Golden Rules

### The 7 Stages of Action

According to Norman (1988), there are seven stages of an action:

1. Have a goal.
2. Make a plan.
3. Specify a set of actions.
4. Perform the set of actions.
5. Perceive the state of the world.
6. Interpret the perception.
7. Compare the outcome with the goal.

Note that (1) to (3) happens within the user's mind, followed by (4) to (7), in which the user acts on this. The seven stages are simplified, and some of the stages occur unconsciously as well. The goals tend to be unconscious as well. Most behaviour does not need to go through all stages in sequence, and most activities will not be satisfied by single actions. There are many numerous feedback loops that occur, and the output of some of those actions are piped into the input of other actions. For many everyday tasks, goals and intentions are not well-specified. Instead, they are opportunistic and unplanned. These actions take advantage of circumstances. This leads us to the *gulf theory*.

### The Gulf Theory
In the seven stages of action, we observe that there are two phases of an action: (1) execution and (2) evaluation. Execution occurs from (1) to (4) followed by evaluation from (5) to (7). There are two gulfs that may occur in these two phases:

1. The *gulf of execution* occurs when there is a mismatch between the user's intentions and the allowable actions.
2. The *gulf of evaluation* occurs when there is a mismatch between the system's feedback/representation and the user's expectations.

## Design Principles

The end-goal of all design principles is to minimise users' cognitive loads and decision-making time.

There are a few important design principles:

1. Ensure consistency: similar situations require similar sets of actions. This reduces the cognitive load for the user to go through the entire process of cognition (see above). Consistency is maintained through a consistent terminology of interface items, including the typography.
2. Exploit familiarity: take advantage of familiar language, symbols, and well-used metaphors.
3. Design affordance: design the interface or product to have affordances and cues that direct the user towards its correct use.
4. Visibility: hidden non-essential functions until they are needed.
5. Feedback: ensure that the state of the system is visible to the user in an unambiguous form.
6. Constraints: limit actions or choices to reduce errors, restrict inputs, and focus the interface on available actions/functions for the user.
7. Recovery: if the system should fail or the user input incorrectly, provide a recovery from the mistake quickly and efficiently.

There are more design principles available [here](https://www.interaction-design.org/literature/topics/design-principles).

## Yielding Closure

A set of actions must terminate. This is what it means to "yield closure." As designers, we should design sequences of actions that are organised into groups that have a clear indication of the beginning and end. Users should then be provided with information about which stage they are at.

The informative feedback at the completion of each group of actions then gives the user the satisfaction of accomplishment. Interactive design can apply the following guidelines:

* Create a clear visual hierarchy.
* Take advantage of conventions.
* Break pages up into clearly defined areas.
* Make it obvious when something can be clicked.

## Specific Applications
