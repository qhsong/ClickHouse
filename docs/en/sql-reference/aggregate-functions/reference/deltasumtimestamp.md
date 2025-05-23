---
description: 'Adds the difference between consecutive rows. If the difference is negative,
  it is ignored.'
sidebar_position: 130
slug: /sql-reference/aggregate-functions/reference/deltasumtimestamp
title: 'deltaSumTimestamp'
---

Adds the difference between consecutive rows. If the difference is negative, it is ignored.

This function is primarily for [materialized views](/sql-reference/statements/create/view#materialized-view) that store data ordered by some time bucket-aligned timestamp, for example, a `toStartOfMinute` bucket. Because the rows in such a materialized view will all have the same timestamp, it is impossible for them to be merged in the correct order, without storing the original, unrounded timestamp value. The `deltaSumTimestamp` function keeps track of the original `timestamp` of the values it's seen, so the values (states) of the function are correctly computed during merging of parts.

To calculate the delta sum across an ordered collection you can simply use the [deltaSum](/sql-reference/aggregate-functions/reference/deltasum) function.

**Syntax**

```sql
deltaSumTimestamp(value, timestamp)
```

**Arguments**

- `value` — Input values, must be some [Integer](../../data-types/int-uint.md) type or [Float](../../data-types/float.md) type or a [Date](../../data-types/date.md) or [DateTime](../../data-types/datetime.md).
- `timestamp` — The parameter for order values, must be some [Integer](../../data-types/int-uint.md) type or [Float](../../data-types/float.md) type or a [Date](../../data-types/date.md) or [DateTime](../../data-types/datetime.md).

**Returned value**

- Accumulated differences between consecutive values, ordered by the `timestamp` parameter.

Type: [Integer](../../data-types/int-uint.md) or [Float](../../data-types/float.md) or [Date](../../data-types/date.md) or [DateTime](../../data-types/datetime.md).

**Example**

Query:

```sql
SELECT deltaSumTimestamp(value, timestamp)
FROM (SELECT number AS timestamp, [0, 4, 8, 3, 0, 0, 0, 1, 3, 5][number] AS value FROM numbers(1, 10));
```

Result:

```text
┌─deltaSumTimestamp(value, timestamp)─┐
│                                  13 │
└─────────────────────────────────────┘
```
