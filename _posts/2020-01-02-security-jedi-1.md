---
layout: post
title: "Security Jedi - Chapter 1: SIEM"
categories: ["Security Jedi"]
tags: ["Big Data", "SIEM"]
author: Nachiket Vartak & Darshan Parab
---

In a previous blog we have covered the motivation for starting this series. In case, you haven't read it yet, I urge you to first read it [here](/security jedi/2020/01/01/security-jedi-introduction.html) before proceeding.

In this, blog we will begin the journey with the most basic requirment of a security analyst, effective monitoring.

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

![Apache Architecture](/assets/images/bigdata.jpg)
<a style="background-color:black;color:white;text-decoration:none;padding:4px 6px;font-family:-apple-system, BlinkMacSystemFont, &quot;San Francisco&quot;, &quot;Helvetica Neue&quot;, Helvetica, Ubuntu, Roboto, Noto, &quot;Segoe UI&quot;, Arial, sans-serif;font-size:12px;font-weight:bold;line-height:1.2;display:inline-block;border-radius:3px" href="https://unsplash.com/@ev?utm_medium=referral&amp;utm_campaign=photographer-credit&amp;utm_content=creditBadge" target="_blank" rel="noopener noreferrer" title="Download free do whatever you want high-resolution photos from ev"><span style="display:inline-block;padding:2px 3px"><svg xmlns="http://www.w3.org/2000/svg" style="height:12px;width:auto;position:relative;vertical-align:middle;top:-2px;fill:white" viewBox="0 0 32 32"><title>unsplash-logo</title><path d="M10 9V0h12v9H10zm12 5h10v18H0V14h10v9h12v-9z"></path></svg></span><span style="display:inline-block;padding:2px 3px">ev</span></a>

### Pretenders and Survivors

While we could write an entire book if we were to cover all the Medusas; but we aren't here to do that! We are here to discuss how you can build something that is tailored for you and your environment

### Sword, Mace, or Light Saber

So, if you are still reading this, that means you are one of us. And, like a true **Jedi** we will forge a path with our light and fight the Dark-side. But wait, what should we use to defeat the **Sith**?

![Elastic&Hadoop](/assets/images/elastichadoop.png)

The choices were many, and choosing a weapon meant, we had to _use it_. And we did, just that. Thank us later!

### Apache - Bees, Elephants, Mahouts and Friends

So, let's start with the _Elephant_ in the room, pun intended. The stack was in place, a team was managing it, it had all the bells and whistles. Would it be love at first sight? Maybe.

![Apache Architecture](/assets/images/bigdatastack.png)

So, we had data that was parsed. A `json` structured stream of data (more on how we did this later). The integration was supposed to be a breeze, would it though?

Kafka chugged down the data like a champ. Now, we needed to store this data. Now that we were using Apache stack, there were quite a few options. We could use HDFS. We could use Hive. We could use Hbase. Since data was already in a json format, we decided that we will go for a No-SQL data store. And Apache HBase was the ultimate choice. We had data in Kafka and it was only the matter of time that we store it in to HBase and start using it for analytics. But how do we send data to Hbase? So we started evaluating our options for ingestion framework.

#### Data Ingestion

As most of the users of Apache Stack would do, we were suggested to go for NiFi. Being a dataflow management platform, It had plethora of options to ingest from, process and forward data to variety of platforms. So we decided to give it a try. But sadly, it could not handle the volume and speed at which the data was being fed and end up being slow.

So we heard there is A Spark and it is supposed to be super fast. Almost 100x faster than other data processing tools and Map-Reduce jobs. Plus, it supported Python. Apperantly it had all the qualities that we were looking for. We decide to give it a try.

Getting started with Spark was not an easy task. To our surprise, Spark couldn't communicate with Kafka. It did not know, how to talks to it and pull or push data to topic. To enable spark to communicate with Kafka,we had to tell it, at runtime, which external java archives to load in order to be able to read data from and push data to topic. And once we did that, we saw that it was truely fast. And we decided to stick with it for ingesting the data.

#### Data Parsing
We weren't much worried about this. Since data was already in json, with our prior experience with MongoDB, we knew we only had to push it HBase and it would store it. Atleast that is what we believed would happen. But to our surprise, that wasn't the case. Even though, the data is in json format, we had to extract individual fields from it and build the entire structure again. But then we thought, that shuoldn't be a problem. Being python users, we knew, it is not a big challange to convert json formatted string to a json object. Spark should be able to do that. And Spark could, but only if you are reading the data from json file and not from Kafka topic. So now we had to build schema for all our data sources and tell Spark to use s specific schema to parse specific data. Imagine doing this for tens of different data sources and each data source requiring multiple json schemas to parse the infomration.

#### Structured Data Storage

Now that we are done with parsing, we can store the data to HBase. But there is a catch. HBase cannot infer schema from the data and it needs the table to be created beforhand with all the columns families and columns qualifiers that could possibly be present in the data. And people who use HBase, they never interact with it directly. As a best practice, they user Apache Phoenix, which presents a HBase's No-SQL tables and RDBMS tables, where colums are not interleaved as families and quilifiers. Instead, you get to see a traditional, flat RDBMS table structure. This beats the whole purpose of having a No-SQL database, as you will have to treat it as RDBMS data store. Also if we just start sending the data to HBase directly, then it will store data in one of the servers in the cluster and not in the distributed manner as we intended it to. This will affect the speed while quering the data and will not be fault tolerent. If server goes down, all the data will be lost. In order the data to be distributed across cluster, we had to create additioncal columns based upon which the data will be distibuted while being stored. Apperantly, using HBase involved a lot of engineering overhead where we did not want to invest our time in. So We dropped the idea of using HBase as our storage.

Now That we realized, we had to use an SQL-like database, we decided to stick with time-tested and proven good technology and Hive became our preffered choice of storage to store structured data.

#### Data Visualization

Hadoop as a Platform had two options for data visualization :
1. Zoomdata
2. Superset

We looked at zoomdata and Superset, and decided to go with Superset looking at its large base of visualization libraries. We started building visualizations and dashboards. But it was very slow. We connected our visualizations to Hive tables and as data started growing the response from hive started getting slower. Again, the efforts to build fast analytics went in vein. Then we came to know about Druid. This is suppose to be a weapon of choice if you want to do analytics on big data. So now we had to create "Hypercubes" from our data, which is basiccaly data aggregation based upon fields to monitor and metrics to be measured. But the problem with Druid is, it doesn't have an access control mechanism. Once the data is indexed, anybody can make web api calls and access the data. To avoid this we had to create external tables in hive, which points to an index in Druid. And there should be no other way to access indexes from Druid. This allowed us to leverage access control from hive to safeguard data in Druid. Now that this problem is solved, we can start sending data to Druid. But again, neither Hive and send data to Druid nor Druid can read data from Hive. We had to write Spark jobs that will pickup data from Hive internal table and push it to hive external table pointing to Druid index.

> Add closure explaining efforts that involved in onboarding the data and number of componants to be managed to keep the pipeline up and running

## Humble Beginnings

After, the earlier nightmare, you might wonder. Was it a right choice to join the **Jedis**. Maybe the **Sith** were right, the dark-side is the way to go.

But there was already something we were using. A tool for automating the `Vulnerability Management` platform. Something that was easy to start with and use. Would it scale though? A nagging question, but only one way to find out; try it!

> **Message from Yoda**: May the force be with you!

## Elastic - A Young Jedi

![Elastic stack](/assets/images/elasticstack.png)

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

![VM Architecture](/assets/images/architecture.png)

We were pleasantly surprised with how easy it was to get the stack up and running. The possibilities were endless and we were about to embark on a journey that would transform the way we monitored our security events.

In the next section we will cover these humble beginnings step-by-step. Until next time.
 
**And, always remember**:

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
