---
title: Database Sys Lec2 Intermediate SQL
date: 2021-01-21 14:09:02
tags:
- Database System
categories:
- Computer Science
---

# Lecture #2: Intermediate SQL

## RELATIONAL LANGUAGES

User only needs to **specify the answer that they want**, not how to compute it. 

The DBMS is responsible for **efficient evaluation of the query**. 

→ High-end systems have a **sophisticated query optimizer** that can **rewrite queries and search for optimal execution strategies**.

- Data Manipulation Language (DML) 
- Data Definition Language (DDL) 
- Data Control Language (DCL)

Also includes: 

→ View definition 

→ Integrity & Referential Constraints 

→ Transactions

Important: SQL is based on **bags (duplicates)** not **sets (no duplicates)**.



## Outline

- Aggregations + Group By 
- String / Date / Time Operations 
- Output Control + Redirection 
- Nested Queries 
- Common Table Expressions 
- Window Functions



## Aggregates

Functions that return a single value from a bag of tuples: 

→ AVG(col)→ Return the average col value. 

→ MIN(col)→ Return minimum col value. 

→ MAX(col)→ Return maximum col value. 

→ SUM(col)→ Return sum of values in col. 

→ COUNT(col)→ Return # of values for col.



**Aggregate functions** can (almost) only be used in the **SELECT output list**.

{% asset_img Aggregates_1.png %}



**DISTINCT** **AGGREGATES**: COUNT, SUM, AVG support DISTINCT

{% asset_img Aggregates_2.png %}



Output of **other columns outside of an aggregate is undefined**.

{% asset_img Aggregates_3.png %}



## **GROUP BY**

**Project tuples into subsets** and **calculate aggregates** against each subset.

{% asset_img Group By_1.png %}



**Non-aggregated values** in **SELECT output clause** must appear in **GROUP BY clause**.

{% asset_img Group By_2.png %}

{% asset_img Group By_3.png %}



## HAVING

**Filters** results based on aggregation computation. Like a **WHERE** clause for a **GROUP BY**

{% asset_img Having_1.png %}



## STRING OPERATIONS

{% asset_img String Operation_1.png %}

**LIKE** is used for string matching. 

String-matching operators 

→**'%' Matches any substring (including empty strings).** 

→**'_' Match any one character**

{% asset_img String Operation_2.png %}

SQL-92 defines string functions. 

→ Many DBMSs also have their own unique functions 

Can be used in either output and predicates

{% asset_img String Operation_3.png %}



SQL standard says to use **||** operator to **concatenate two or more strings together**.

{% asset_img String Operation_4.png %}



## DATE/TIME OPERATIONS

Operations to manipulate and modify **DATE/TIME attributes**. 

Can be used in either **output and predicates**. 

Support/syntax varies wildly…



## OUTPUT REDIRECTION

Store **query results** in another table: 

→ **Table must not already be defined**. 

→ Table will have the **same # of columns** with **the same types as the input**.

{% asset_img Output Redirection_1.png %}

Insert tuples from query into another table: 

→ Inner SELECT must generate the same columns as the target table. 

→ DBMSs have different options/syntax on what to do with integrity violations (e.g., invalid duplicates).

{% asset_img Output Redirection_2.png %}



## OUTPUT CONTROL

**ORDER BY** <column*> [ASC|DESC]

→ Order the output tuples by the values in one or more of their columns.



**LIMIT**  **<count> [offset]** 

→ Limit the # of tuples returned in output. 

→ Can set an offset to return a “range”



## NESTED QUERIES

Queries containing other queries. 

They are often difficult to optimize. 

**Inner queries can appear (almost) anywhere in query**.



**ALL**→ Must **satisfy expression for all rows** in the sub-query. 

**ANY**→ Must **satisfy expression for at least one row** in the sub-query. 

**IN**→ **Equivalent to '=ANY()'** . 

**EXISTS**→ At **least one row is returned**.



## WINDOW FUNCTIONS

Performs a "sliding" calculation across a set of tuples that are related. 

Like an aggregation but tuples are not grouped into a single output tuples.

{% asset_img Window Function_1.png %}

{% asset_img Window Function_2.png %}

The **OVER** keyword specifies how to **group together tuples** when computing the window function. 

Use **PARTITION BY to specify group**.

{% asset_img Window Function_3.png %}

You can also include an **ORDER BY in the window grouping** to sort entries in each group.

{% asset_img Window Function_4.png %}



## COMMON TABLE EXPRESSIONS

Provides a way to write **auxiliary statements for use in a larger query**. 

→ Think of it like a **temp table just for one query**. 

Alternative to nested queries and views.

{% asset_img Window CTE_1.png %}

{% asset_img Window CTE_2.png %}