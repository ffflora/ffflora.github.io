---
title: "CDP OR Reverse ETL, one or both?"
date: 2022-03-10T23:18:51-08:00
draft: true
toc: false
images:
tags:
- ETL 
categories:	
- Data Analytics
- Data Science

---

By definition, 

> ETL is a data integration process that synchronizes multiple data sources into a single destination, such as a data warehouse. It stands for extract, load, and transform, which are the three steps that occur in the process 

> **Extract:** Take the desired data from numerous tools and sources

> **Transform:** Clean (or reorient) the data into a standard format to store it in 

> **Load:** Put the transformed data into organized tables that will live inside a data warehouse

Reverse ETL, which is the opposite of traditional ETL process, are the tools take data from the centralized data warehouses to different downstream systems such as BI tools, marketing/sales analysis tools, etc. It sounds handy enough to supply and transfer the data in between different platforms, but it doesn’t usually provide an interface to activate data into personalized, targeted, cross-channel customer experiences, therefore it lacks the insights in customer aspects. 

In such cases, CDP could come into play, to fill in the gap. CDP provides profiles for customers based on the combined user data from potentially different data sources, which could save a huge amount of time and engineering effort. CDP makes it possible and accessible for no-coding teams, and mainly focus on the marketing/sales aspect of data orchestration, while Reverse ETLs contribute in maintains and moving data within different data warehouses, data streams, analytical results, and other data platforms, and therefore it requires more engineering efforts. 
I made a horizontal comparison with Reverse ETL tools(mParticle and Treasure Data) and CDP tools(Census and RudderStack) via the website Slashdot ([full comparison here](https://slashdot.org/software/comparison/mParticle-vs-Treasure-Data-vs-Census-vs-RudderStack/)), to have a more straightforward understanding of the tools, and then found some of the functionalities overlap within and cross the category. In terms of pricing, CDP tools are usually based on the number of visits per month, and Reverse ETL tools are usually based on the number of downstream applications used. For example, RudderStack has a free version, and its business plan starts at $750/month, which is a bit more economical solution compared to the Census, which the business plan starts at $800/month. 

Therefore, 

1. If the client doesn’t heavily rely on the marketing/sales/any other user-based data, nor analysis that comes from such groups, and/or they have enough data engineers/data scientists resources, I would recommend just implementing the Reverse ETL solution; and in our case within the comparison, RudderStack would be a better choice because it supports the ingratiation for Braze and Snowflake, and the pricing is more competitive to Census. With all data centralized in a warehouse, the client can generate more insights, predictions, forecasts for different purposes; and since it has more integrations, the client can benefit from the convenience of getting data. It just takes more time and resources on engineering, if needed.
2. If the client’s first primarily is user-based marketing, and if the team is more non-technical, and there’s not a massive amount of data every month, I would recommend the client implement the CDP tools only. The client can benefit from using CDP to manage customer data, perform cross-channel analysis, personalization analysis, etc. 
3. If the client has both marketing team and data analytics/engineering team, or if the client has such needs, or, if the customer data is in a great volume that is not efficient to process all at once by CDP only, then a combination of CDP and Reverse ETL together would be recommended.
