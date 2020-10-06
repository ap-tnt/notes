---
layout: post
title:  "Examples of Rowcounts in Operators"
date:   2020-10-01 16:30:52 +0100
categories: execution plans
tags: [Execution Plans, SQL]
# bundle exec jekyll serve --livereload
---
I quite often get confused with rowcounts in execution plans, so this page will be a load of examples that I can refer back to.


### Example 1
```sql
create index displayname on users (displayname)

select Id, DisplayName, Location
from users
where DisplayName = 'chris'
and location <> 'San Francisco, CA'
```
\
And here's the execution plan:

![](/notes/images/2020-10-06-08-05-20.png)

- This query returns 253 rows
- There are 926 Chris's

Index seek is easy:

![](/notes/images/2020-10-06-08-13-28.png)

The key lookup is more interesting:

![](/notes/images/2020-10-06-08-16-58.png)
![](/notes/images/2020-10-06-08-17-50.png)