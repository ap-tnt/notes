---
layout: post
title:  "Notes on Execution Plans"
date:   2020-10-01 06:26:52 +0100
categories: jekyll update
tags: [Execution Plans, SQL]
# bundle exec jekyll serve --livereload
---

What do the thickness of the lines mean in an *ACTUAL* execution plan in SSMS?

How much data SQL Server needed to read, in order to return those results (see number of rows read in the execution plan operator)
\
\
\
What do the thickness of the lines mean in an *ESTIMATED* execution plan in SSMS? 

How much data SQL Server thinks that it needed to read (i.e. an estimate) in order to return those results (see number of rows read in the execution plan operator)
\
\
\
Number of rows read in an operator relates to the object that the operator specifies. So the example below is 926 rows read from the object, which is StackOverflow2010.dbo.Users.PK_Users_ID. Also note how SentryOne doesn't call it 'object', but it still specifies what the thing is we are looking at:
![](/notes/images/2020-10-06-08-16-58.png)
