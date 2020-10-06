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

- The SSMS key lookup operator said 253 of 563708. That's 253 results coming from that operator (i.e. Chris + <> San Francisco, CA), and 563708 estimated number of rows for all executions.
- The 563708 figure comes from estimated number of rows per execution * number of exections, or 608.756 * 926 = 563,708.056.
- Plan Explorer does not show estimated number of rows for all executions.
- I'm not sure yet where estimated number of rows per execution comes from (608.756), but I think it's coming from the stats.
- Estimated number of rows per execution in SSMS is what Plan Explorer calls Estimated Rows.