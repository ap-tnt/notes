---
layout: post
title:  "Does Updating Stats cause a Recompile?"
date:   2020-11-26 09:30:52 +0100
categories: SQL
tags: [Execution Plans, SQL, Statistics]
# bundle exec jekyll serve --livereload
---

This came up recently and I had to look it up. I know that I will forget it, so I am making a note here.

The short answer is: not always.

The things that need to be considered are:

- Has any data in the table changed?
- What version of SQL Server are we talking about?
- Is the plan trivial or not?
- Is auto update stats enabled?

If the plan is trivial, or if no data has changed, the plan will not recompile. this page has an excellent walkthrough example https://techcommunity.microsoft.com/t5/sql-server-support/does-statistics-update-cause-a-recompile/ba-p/318545

This Kendra Little page also has some good examples https://www.brentozar.com/archive/2015/01/updating-statistics-cause-recompile-no-data-changed/. It focuses mostly on the data changing. But there's a comment from James Lupolt that says prior to 2012, statistics updates would cause plan invalidation as long as auto update stats was enabled.
