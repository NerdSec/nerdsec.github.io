---
layout: post
title: "Security Jedi - Chapter 2: Vulnerability Management"
categories: ["Security Jedi"]
tags: ["Big Data", "Vulnerability Management"]
author: Nachiket Vartak & Darshan Parab
---

In a previous blog we have covered the motivation for starting this series. In case, you haven't read it yet, I urge you to first read it [here](/security jedi/2020/01/01/security-jedi-introduction.html) before proceeding.

Today, we will cover the origins of our **Cyber Security Analytics** platform. The first use case for implementing this stack was actually quite different. Vulnerability Management.

## Vulnerability Management Automation

Vulnerabilities are the biggest issue in Cyber Security. There are almost [30-40](https://www.cvedetails.com/browse-by-date.php) vulnerabilities discovered each day. Managing these, in an environment with a few thousand assets can seem quite daunting. There is simply too much data. Many times, a few of these vulnerabilities fall through the cracks of patch management and then are later used in a `Red Team` engagement. Yes, we have all been there.

So, we decided to automate the entire excercise. What if we could put the ownership of all these vulnerabilities on the individual stakeholders? Let them have a view of how horrible the situation is for their assets.

## The Beginning

We started with a quite simple architecture. We used the native [report publishing](https://docs.tenable.com/sccv/Content/PublishingSitesSettings.htm) feature present in Tenable. This allowed us to send the Vulnerability data over `https` to a webserver.

![VM Architecture](/assets/images/architecture.png)

We then used Logstash [http input](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-http.html), to receive this data and then forward it to Elasticsearch. Then it was a simple matter of visualizing the data in Kibana and showing it to the world (just kidding).

## Issues

Although visualizing this data seems quite straight-forward and easy, there are some challenges with this approach:

1. Context

    The data clearly lacks context from a Business perspective. It limited to only vulnerability specific information and not really any information that makes it relevant to my environment.

2. 

But, I don't have/use **Tenable Security Center**. Well, then you could be using Nessus, OpenVAS, or 

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
