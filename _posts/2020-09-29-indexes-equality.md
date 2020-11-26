---
layout: post
title:  "Indexing for Equality Searches"
date:   2020-09-29 16:26:52 +0100
categories: SQL
tags: [Indexes, SQL]
---
Before we get into that, here is a top tip. To visualise an index, do a select query that matches its structure (including the order by). You should get an index scan in the execution plan, and NOTHING else.\
\
So here is our query, and we want to create the perfect index for it:

```sql
select Id, DisplayName, Location
from users
where DisplayName = 'chris'
and location = 'San Francisco, CA'
```
\
Consider this index:

```sql
create index displayname_includes on users (displayname) include (location)
```
\
This looks good from the execution plan. You get an index seek, so it should be great. Here's the index seek operator:

![](/notes/images/2020-09-29-17-07-40.png)

Note that the rows read is 831, but the actual number of rows read is 4. There are:

- 926 Chris rows
- 1,660 rows of San Francisco, CA
- 5 rows of Chris + San Francisco, CA

\
So, SQL server had to read all the Chris rows in the index, and then go through each of those and check for San Francisco, CA, because the included column of location was not sorted. Another thing to notice is that you can see the seek predicates - this is the DisplayName = 'Chris' part. You can also see the predicate where location = @2. This is also known as a residual predicate - I think it's like another filter on your filter, or a scan of whatever comes back from your index seek. These are things to look out for, as it might look like an index seek, but it's behaving like a scan. You can understand it better if we visualise the index, which would look like this:

![](/notes/images/2020-09-29-17-13-04.png)\
\
\
Now let's consider another index:

```sql
create index displayname_location on users (displayname, location)
```
\
If we visualise that index, it would look like this:

![](/notes/images/2020-09-29-17-27-48.png)
\
\
The operator now looks like this:

![](/notes/images/2020-09-29-17-30-26.png)
\
\
No residual predicate, and number of rows read is perfect. 

All the indexes are good, but you need to check the logical reads to see which is best:

- displayname_includes = 7 logical reads
- displayname_location = 4 logical reads
- location_displayname = 3 logical reads
 
\
So you need to figure out how useful each index will be for everything else you have, and how perfect you want the index to be for this particular query. These will have a bearing on it:

- Do we query on displayName and location all the time?
- Do we query on displayName for some queries and displayName and location for others? I.e. never on just location 
- Do we query on location for some queries and displayName and location for others? I.e. never on just displayName 
- If it's equality searches, the order of the keys doesn't really matter.