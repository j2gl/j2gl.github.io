+++
date = '2021-05-13T19:34:42+02:00'
draft = false
title = 'Filter jsonb Array Data in PostgreSQL'
tags = ['sql', 'postgresql', 'jsonb']
theme = 'PaperMod'
+++

## Filtering `jsonb` Array Data in PostgreSQL

One day, I faced a challenge: how do you filter rows in a PostgreSQL table when the data you want to query lives inside a `jsonb` array?

Let me walk you through the solution step by step.

---

## The Setup

We will ll start by creating a temporary table called `table1` with some JSON data stored in a column named `dummy_data`:

```sql
CREATE TEMP TABLE table1 AS
  SELECT id::int, dummy_data::jsonb FROM ( 
    VALUES 
      (1, '[
          {"code": "AB123456", "other": "xxx",  "amount": 10, "step": "STEP_1"},
          {"code": "HH654654", "other": null,   "amount": 20, "step": "STEP_2"}
       ]'),
      (2, '[
          {"code": "45648asd", "other": null,   "amount": 30, "step": "STEP_1"},
          {"code": "QWERASDF", "other": "asdf", "amount": 40, "step": "STEP_2"}
       ]'),
      (3, '[]'),
      (4, '[
          {"code": "POIUASDF", "other": null,   "amount": 50, "step": "STEP_1"},
          {"code": "SFDTGFDJ", "other": null,   "amount": 60, "step": "STEP_2"},
          {"code": "DFGERUDD", "other": "1234", "amount": 70, "step": "STEP_3"},
          {"code": "34567DTH", "other": null,   "amount": 80, "step": "STEP_4"}
       ]')
  ) AS t(id, dummy_data);
```

This creates a table with data similar to the following:

```text
id | dummy_data
---|---------------------------------------------------------------------
 1 | [{"code": "AB123456", "step": "STEP_1", ...}, {"code": "HH654654", "step": "STEP_2", ...}]
 2 | [{"code": "45648asd", "step": "STEP_1", ...}, {"code": "QWERASDF", "step": "STEP_2", ...}]
 3 | []
 4 | [{"code": "POIUASDF", "step": "STEP_1", ...}, {"code": "SFDTGFDJ", "step": "STEP_2", ...}, ...]
```

---

## Goal

We want to filter the rows and extract only those JSON elements where `"step"` is either `"STEP_1"` or `"STEP_4"`.

---

## Step 1: Flatten the `jsonb` Array

Use `jsonb_array_elements()` to expand each JSON array into individual rows.

```sql
SELECT 
  id, jsonb_array_elements(dummy_data) AS steps
FROM 
  table1;
```

Result:

```text
id | steps
---|-----------------------------------------------------------
 1 | {"code": "AB123456", "step": "STEP_1", ...}
 1 | {"code": "HH654654", "step": "STEP_2", ...}
 2 | {"code": "45648asd", "step": "STEP_1", ...}
 2 | {"code": "QWERASDF", "step": "STEP_2", ...}
 4 | {"code": "POIUASDF", "step": "STEP_1", ...}
 4 | {"code": "SFDTGFDJ", "step": "STEP_2", ...}
 4 | {"code": "DFGERUDD", "step": "STEP_3", ...}
 4 | {"code": "34567DTH", "step": "STEP_4", ...}
```

📚 See the official docs: https://www.postgresql.org/docs/current/functions-json.html

---

## Step 2: Filter the Desired Steps

Now that the data is flattened, we can filter it using a `WITH` clause and a `WHERE` condition.

```sql
WITH expanded AS (
  SELECT 
    id, jsonb_array_elements(dummy_data) AS steps
  FROM 
    table1
)
SELECT 
  id, steps, steps ->> 'step' AS step_value
FROM 
  expanded
WHERE 
  steps ->> 'step' IN ('STEP_1', 'STEP_4');
```

Result:

```text
id | steps                                                 | step_value
---|--------------------------------------------------------|------------
 1 | {"code": "AB123456", "step": "STEP_1", ...}            | STEP_1
 2 | {"code": "45648asd", "step": "STEP_1", ...}            | STEP_1
 4 | {"code": "POIUASDF", "step": "STEP_1", ...}            | STEP_1
 4 | {"code": "34567DTH", "step": "STEP_4", ...}            | STEP_4
```

---

## Conclusion

Filtering `jsonb` arrays in PostgreSQL becomes straightforward once you flatten the array using `jsonb_array_elements()` and apply standard SQL filtering logic.

