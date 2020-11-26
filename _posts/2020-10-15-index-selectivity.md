---
layout: post
title:  "Index selectivity"
date:   2020-10-15 06:26:52 +0100
categories: SQL
tags: [Indexes, SQL]
---
Selectivity is not about how unique the data in the table is. Selectivity is about the contents of your query. 

Consider scenario 1
Should you index a gender column in the database? First instinct is no because it hardly get's any less selective in terms of the data in the table. But if you are looking WHERE gender = 'alien', then selectivity becomes very high. Not about the data in the table - it's your WHERE clause that governs selectivity.

Consider scenario 2
Should you index date_created column? First instinct is yes because it's highly selective. But what if your query says WHERE date_created < today? That's going to bring everything back. So this is the reverse of scenario 1 - the data in the table is very selective, but our WHERE clause has made it not selective.