---
layout: post
title: "Security Jedi Series - Introduction"
categories: ["Beginner's Guide"]
tags: ["Big Data", "SIEM"]
---

Security is an ever evolving field, where the threats and its detection mechanisms are ever evolving. In such a field the pressure on the `Blue Team` is far greater than that on the adversary. For, as a defender, the `Blue Team` has to always get it right; while the adversary has to get it right just once!

## Problems Compounded

In such a scenario, a defender needs to make his job as simple and fast as he can. Traditionally, in a SOC environment the detection responsibilities have fallen on the shoulders of the good ol' **SIEM**. But they typically have limitations surrounding:

1. Scalability
2. Search

### Scalability

As the volume of data gets larger the robustness of a platform built on a custom DB or a traditional RDBMS takes a hit. Plus it get really expensive to buy special appliances or pay for an additional license to handle that extra data.

### Search

The dreaded `Search` functionality is an eye sore for most **SIEM** products on the market today. You either have to learn a custom query language or rely on columnar search approach, which should be fine, if implemented correctly. Alas, it isn't (in our expereience)!

Unfortunately, most of this `Search` functionality is monolithic and feels quite limiting as an analyst. This is especially true when you just have to run a quick raw text search.

## Problems Solved

Having experienced these problems first hand, we decided to do something about it. We realized that what did work for us, in a traditional SOC, will no longer work for us in the future. We needed a platform that would empower the Analyst to start with a blank canvas and then paint his picture with ease. We needed to empower the analyst to be able to do _analytics_. Having learned from our SIEM experience, we sat down and chalked down our fundamental `Design Principles`. Our new _**Cyber Security Analytics**_ platform would adhere to these principles and hopefully overcome our **SIEM** shortcomings.

### Design Priciples

1. Open Source - Community driven
2. Strong REST API
3. Scalable
4. Search - Fast query execution
5. Intuitive

## A Long Journey

It is one thing to decide to build our own **Cyber Security Analytics** platform, and another to actually do it. The biggest challenge that we faced was identifying the technology around which we would build this stack.

This blog is a series of posts that covers this journey. It is a highly subjective reflection of our thoughts and ideas and in no way reflects any other entity or organization. We hope that this series turns out to be of some help to someone who wants to emulate this journey or some portion of it. In the next blog we will cover the humble beginnings of our stack and why we chose it.

> Within infinite myths lies the eternal truth
>
> Who sees it all?
>
> Varuna has but a thousand eyes,
>
> Indra has a hundred,
>
> You and I, only two.
>
> -Devdutt Patnaik

![Read Chapter 1 - here]()