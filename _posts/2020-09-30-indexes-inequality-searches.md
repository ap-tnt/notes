---
layout: post
title:  "Indexing for Inequality Searches"
date:   2020-09-30 16:26:52 +0100
categories: SQL
tags: [Indexes, SQL]
---
Now let's consider this query:

```sql
select Id, DisplayName, Location
from users
where DisplayName = 'chris'
and location <> 'San Francisco, CA'
```
\
I am going to create 3 indexes and compare them all:

```sql
create index name_includes on users (displayname) include (location)

create index name_loc on users (displayname, location)

create index loc_name on users (location, displayname)
```
\
Here's the queries that I will test:
```sql
select Id, DisplayName, Location
from users with (index = 1) --(i.e. a clustered index scan)
where DisplayName = 'chris'
and location <> 'San Francisco, CA'


select Id, DisplayName, Location
from users with (index = name_includes)
where DisplayName = 'chris'
and location <> 'San Francisco, CA'


select Id, DisplayName, Location
from users with (index = name_loc)
where DisplayName = 'chris'
and location <> 'San Francisco, CA'


select Id, DisplayName, Location
from users with (index = loc_name)
where DisplayName = 'chris'
and location <> 'San Francisco, CA'
```
\
Here's a table that compares the total pages in the index to the amount of logical reads each query does, so that we can see how much of the index is being read: 
<!--- https://www.tablesgenerator.com/markdown_tables --->

| Index         | Logical Reads | Total Pages in the Index |
|---------------|---------------|--------------------------|
| PK_Users_ID   | 7,405         | 7,427                    |
| name_includes | 7             | 1,576                    |
| loc_name      | 957           | 1,627                    |
| name_loc      | 9             | 1,581                    |

\
If we look at the execution plans for the last two, (loc_name vs name_loc), notice how they are both index seeks:

![](/notes/images/2020-10-12-13-48-32.png)

You have to start digging into the operators to understand what's going on:

Name_loc:

![](/notes/images/2020-10-14-08-44-01.png)
\
\
Loc_Name:

![](/notes/images/2020-10-12-13-55-02.png)

Also, see how Plan Explorer makes it more obvious that there is a problem:

![](/notes/images/2020-10-14-08-48-23.png)

I think we're getting the residual predicate thing going on again. The only thing I'm not sure about is that the predicate in my [example on indexing for equality](2020-09-29-indexes-equality.md) had an @2 in it, and this one doesn't.