---
layout: post
title: "Design Google like Typeahead Search"
date: 2018-07-30T18:27:10-07:00
categories: "article interview design"
tags: search google trie algomeetup
---
## Problem
Design "Typeahead suggestions enable users to search for known and frequently searched terms. As the user types into the search box, it tries to predict the query based on the characters the user has entered and gives a list of suggestion to complete the query. Typeahead suggestions help the user to articulate their search queries better. It’s not about speeding up the search process but rather about guiding the users and lending them a helping hand in constructing their search query."

Credits to [educative.io](https://www.educative.io/collection/page/5668639101419520/5649050225344512/5076324926357504) for problem definition and actually great course.

![Google Search Suggestions screenshot](/assets/google_search_suggestions.png "Something like this")

## Assumptions
+ no spellchecking
+ no personalization
+ not specific to region
+ english only
+ no lexical analysis

## Solution #1
1. Collect the queries in advance, ranged by popularity and limited in the amount. (here Map-Reduce could be used for processing query logs for example)

  technically search queries like
  "design"
  "design google search"
  "design thinking"
  "design patent"
  "design patent search google"

1. Build try structure for this data in trie way
1. Go recursively via the tree and put everything to the REDIS cache

##### Structure update
Every day rebuild structure based on requests received during the day

*N.B. Trie data structure is perfect in terms of optimization of computation logic, however let us think about it in when we have networking interaction in the picture*

*N.B. Google does in 243ms first time  (DNS resolution, TLS handshake) and 128ms every next time.
It used some of edge nodes for this (I was in Palo Alto that time)
172.217.7.132	United States 	California	Mountain View*

*N.B. Google sends request every edit field changed*

##### Term lifecylce
It is suggested to add terms with TTL like 2 days, however recompute them every day.
So, the term would be either overwritten for most cases or destroyed by caching system itself

##### Deployment
Data locality is important, edge deployments are suggested.
Split horizon DNS should be also in place.

###### Sharding and partitioning
We probably want to shard our data in case we we have too much of it for one machine.
Using hash partitioning sounds reasonable, taking into account that some of prefixes are more popular that others
**[More about Redis partitioning][redis_partitioning]**.

In order to avoid spikes in some of shards it is suggested to store data by hash (xxHash is known as fast as memory access https://cyan4973.github.io/xxHash/)
In this case data structure in cash should contain a map of pairs in case of collision

[redis_partitioning]: https://web.archive.org/web/20180507055057/https://redis.io/topics/partitioning
