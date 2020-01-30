---
layout: post
title: "Security Jedi Series - Chapter 1"
categories: ["Beginner's Guide"]
tags: "Big Data"
---

## SIEM - Hero or Villain?

For the past decade, **SIEM** has been ruining the lives of many SOC Analysts. Just like **Anakin Skywalker**, all **SIEM** technologies started with good intentions. But how over time things have changed. A tool meant to ease the life of an Analyst has now turned out to be his life's biggest bane.

Ask any analyst to use only a commercial **SIEM** in the market today for his monitoring activities and you can watch him turn to stone from fear. Literally!

### Performance Tuning

Maybe the introduction was a bit over the top (or maybe not). But, the general consensus over **SIEM** is that eventually, it spirals out of control. _Does it have to though?_

Maybe not, but it's not easy. A well maintained SIEM requires a considerable amount of the following:

* Effort
* Resources
* Time
* Money

These four things, if you haven't noticed, are not abundant in our field. Also, in our field of work, ask any SOC Analyst; what do you want from your monitoring platform? And I can guarantee you that the answer would boil down to these points:

* Speed
* Flexibility
* Ease of use

Our **SIEM**, was quite easy to use. I will concede that much. But, boy was she rigid, and don't even get me started on the speed. Run a query now and **expect a response once the dust on your grave is settled**. But then again, it was getting old with little maintanance. Not that when it had both, it did any better, but still. I digress.

## Luke Skywalker - Big Data Fame

Then came the oracle, use a big data stack to solve this problem. Some **SIEM**, tried to jump on this band wagon and disguise themselves as a Big Data platform. Some tried to sell themself as an augmented solution sitting alongside your **Big Data** stack. Some ignored it, hoping that you would forget about it and finally see the light (or darkness). While some platforms truly did make that switch or better still, were designed with those principles in mind.

    insert big data image

### Pretenders and Survivors

While we could write an entire book if we were to cover all the Medusas; but we aren't here to do that! We are here to discuss how you can build something that is tailored for you and your environment

### Sword, Mace, or Light Saber

So, if you are still reading this, that means you are one of us. And, like a true **Jedi** we will forge a path with our light and fight the Dark-side. But wait, what should we use to defeat the **Sith**?

The choices were many, and choosing a weapon meant, we had to _use it_. And we did, just that. Thank us later!

    image of elastic plus other technologies

### Apache - Bees, Elephants, Mahouts and Friends

    image of apache stack

So, let's start with the _Elephant_ in the room, pun intended. The stack was in place, a team was managing it, it had all the bells and whistles. Would it be love at first sight? Maybe.

So, we had data that was parsed. A `json` structured stream of data (more on how we did this later). The integration was supposed to be a breeze, would it though?

Kafka chugged down the data like a champ. Now, we needed to store this data

Since data is already in json format we decided to user No-SQL database to store the data assuming that it will be schemaless and we don't have to define anything before we start storing the data. But sooner, we realized, that is not the case. We still have to define data structure. Moreover We have to put effort in engineering so that data is equally distributed across data storage. Then we have to use a middle layer which makes No-SQL looks like SQL to simplify data ingestion. So we gave up on No-SQL storage and decided to use Hive since it is more established and was in use by other teams. But what we wanted to do is to use the data for anlytics and apperently hive was very slow for this task. So now we have to use Druid in order to 
have analytics at speed and to visualize it with Superset. Next task was to take data from hive table and send it to Druid index where it will be aggregated before storing. One simple task of ingesting the data and visualizing it involved so many componants and intricacies.

Kafka -> HBase

Challenges:

1. Engg efforts
2. Pheonix - Define a mapping -> back to RDBMS?
3. Why not HIVE if mapping to be done. More established and already in use by teams.
4. But analytics not efficient in HIVE
5. DRUID for analytics. -> Spark job to aggregate and send data to DRUID
6. Superset for visualizing data in DRUID.

## Humble Beginnings

After, the earlier nightmare, you might wonder. Was it a right choice to join the **Jedis**. Maybe the **Sith** were right, the dark-side is the way to go.

But there was already something we were using. A tool for automating the `Vulnerability Management` platform. Something that was easy to start with and use. Would it scale though? A nagging question, but only one way to find out; try it!

> **Message from Yoda**: May the force be with you!

## Elastic Snake

    Elastic image here

Like all good superhero movies, where a hero has humble beginnings, our story has a similar outline. Aggregating `Vulnerability Management` data (`VM`) was a bit of a challenge using the traditional RDBMS systems. It just does not scale when you have a few thousand servers with a gazillion vulnerabilities.

MongoDB was a great alternative, but it lacked on two fronts for us:

* Shards
* Visualiztions

Getting these two up and running was not a pleasant experience in all honesty. We needed something with little management and immediate results.

Elastic stack was a perfect alternative. It had all the needed features:

1. Easy to setup
2. Easy to use
3. Easy to scale

So we started with a humble architecture:

    image of initial architecture

We were pleasantly surprised with how easy it was to get the stack up and running. The possibilities were endless and we were about to embark on a journey that would transform the way we monitored our security events.

In the next section we will cover these humble beginnings step-by-step. Until next time.

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
