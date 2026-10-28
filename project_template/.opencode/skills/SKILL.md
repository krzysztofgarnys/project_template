---
name: ef-core-performance
description: Analyze and improve Entity Framework Core query performance, database access patterns, tracking, N+1 problems, indexes and transaction boundaries.
---

# EF Core Performance

Use this skill when analyzing or modifying code that uses Entity Framework Core.

## Before changing code

Inspect:

1. The LINQ query.
2. When the query is materialized.
3. Navigation property usage.
4. Number of database round trips.
5. Tracking requirements.
6. Expected number of rows.
7. Existing indexes.
8. Transaction boundaries.

Do not optimize blindly.

First identify the actual or likely bottleneck.

## Query analysis

Check for:

- N+1 queries
- unnecessary Include()
- premature ToList()
- multiple queries that could be combined
- loading entire entities when projection is enough
- client-side processing
- unnecessary tracking
- large result sets without pagination

Prefer projection:

Select(x => new Dto
{
    Id = x.Id,
    Name = x.Name
})

instead of loading full entities when only a subset of fields is needed.

## Tracking

For read-only queries consider:

AsNoTracking()

Do not use it when entities must later be updated through the same DbContext.

## Async

Use:

- ToListAsync
- FirstOrDefaultAsync
- SingleOrDefaultAsync
- SaveChangesAsync

Propagate CancellationToken.

Do not use:

- .Result
- .Wait()

## DbContext

DbContext is not thread-safe.

Never execute parallel operations using the same DbContext instance.

Bad:

Task.WhenAll(
    db.Users.ToListAsync(),
    db.Orders.ToListAsync()
)

when both operations use the same DbContext.

## Indexes

When a query is slow, inspect:

- WHERE columns
- JOIN columns
- ORDER BY columns
- selectivity
- composite index ordering

Do not recommend an index without explaining which query it supports.

Remember that indexes increase:

- storage
- INSERT cost
- UPDATE cost
- DELETE cost

## Transactions

Keep transactions as short as possible.

Avoid external HTTP calls while holding a database transaction.

Consider optimistic concurrency for competing updates.

## Output

When reviewing a query provide:

1. Problem
2. Evidence
3. Expected impact
4. Recommended change
5. Trade-offs
6. Whether measurement is required