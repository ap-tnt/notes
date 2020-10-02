---
layout: post
title:  "Indexing for Inequality Searches"
date:   2020-09-30 16:26:52 +0100
categories: jekyll update
tags: [indexes, sql]
---
Now let's consider this query:

```sql
select Id, DisplayName, Location
from users
where DisplayName = 'chris'
and location <> 'San Francisco, CA'
```
\
