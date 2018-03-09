---
layout: post
title: Recursively generating keyset pagination queries
key: keyset-pagination
author: Marius Metzger
tags: sql pagination javascript
---
### The problem with offset-based pagination
When paginating an SQL table, the most common approach is using an `OFFSET` clause 
to skip the first `n` results to get to a specific page.
However, this approach **does not scale well** and can't handle rows that were added since the last time a page was fetched.

**Markus Winand** explains this issue very well in 
[his blog](https://use-the-index-luke.com/sql/partial-results/fetch-next-page), and proposes a straight-forward solution:
**keyset pagination**.

### Introducing keyset-based pagination

The idea behind keyset pagination is the following: Instead of **fetching all records** and **discarding** those from previous pages,
we only fetch results that come after the last result of the previous page.
