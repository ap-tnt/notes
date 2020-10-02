---
layout: post
title:  "Examples of Rowcounts in Operators"
date:   2020-10-01 16:30:52 +0100
categories: execution plans
tags: [execution plans, sql]
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

And here's the execution plan:
