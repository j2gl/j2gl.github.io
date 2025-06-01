+++
date = '2021-05-13T19:34:42+02:00'
draft = false
title = 'Filter jsonb array data in PostgreSQL '
tags = ['sql']
theme = "PaperMod"
+++

## What happened?

One day I had the issue on how should I filter data from a table in postgres.   But the catch is the data was 
in a jsonb field, which makes it very different.

Here is the example:


Firs we will create a table called `table1` with json data in `dummy_data` field.

```sql
CREATE TEMP TABLE table1 AS
  SELECT id::int, dummy_data::jsonb FROM ( 
  	values 
    	( 1, '[
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

It will create the temporary table1 with this data:
```json
id|dummy_data                                                                                                                                                                                                                                                     |
--|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
 1|[{"code": "AB123456", "step": "STEP_1", "other": "xxx", "amount": 10}, {"code": "HH654654", "step": "STEP_2", "other": null, "amount": 20}]                                                                                                                    |
 2|[{"code": "45648asd", "step": "STEP_1", "other": null, "amount": 30}, {"code": "QWERASDF", "step": "STEP_2", "other": "asdf", "amount": 40}]                                                                                                                   |
 3|[]                                                                                                                                                                                                                                                             |
 4|[{"code": "POIUASDF", "step": "STEP_1", "other": null, "amount": 50}, {"code": "SFDTGFDJ", "step": "STEP_2", "other": null, "amount": 60}, {"code": "DFGERUDD", "step": "STEP_3", "other": "1234", "amount": 70}, {"code": "34567DTH", "step": "STEP_4", "other|
```

The question is how can I get all the rows that has `"step":"STEP_1"` or `"step":"STEP_2"` in the data.  

### Step 1
First we use this query to transform every item in the array in a row using `jsonb_array_elements`, check the results.

```sql
SELECT 
		id, jsonb_array_elements(dummy_data) as steps
FROM 
    table1

id|steps                                                                |
--|---------------------------------------------------------------------|
 1|{"code": "AB123456", "step": "STEP_1", "other": "xxx", "amount": 10} |
 1|{"code": "HH654654", "step": "STEP_2", "other": null, "amount": 20}  |
 2|{"code": "45648asd", "step": "STEP_1", "other": null, "amount": 30}  |
 2|{"code": "QWERASDF", "step": "STEP_2", "other": "asdf", "amount": 40}|
 4|{"code": "POIUASDF", "step": "STEP_1", "other": null, "amount": 50}  |
 4|{"code": "SFDTGFDJ", "step": "STEP_2", "other": null, "amount": 60}  |
 4|{"code": "DFGERUDD", "step": "STEP_3", "other": "1234", "amount": 70}|
 4|{"code": "34567DTH", "step": "STEP_4", "other": null, "amount": 80}  |
```

For `jsonb_array_elements`, check the official [docs](https://www.postgresql.org/docs/9.5/functions-json.html).


### Step 2

Now that we have all the rows flattened, it's a matter to use a combination of a `WITH` with `WHERE` in the query.

```sql
WITH t1 AS (
    -- The query from step 1
	SELECT 
		id, jsonb_array_elements(dummy_data) as steps
	FROM 
		table1
) 
SELECT 
	id, steps, steps ->> 'step'
FROM 
	t1
WHERE 
	steps ->> 'step' = 'STEP_1'
	OR steps ->> 'step' = 'STEP_4'

id|steps                                                               |?column?|
--|--------------------------------------------------------------------|--------|
 1|{"code": "AB123456", "step": "STEP_1", "other": "xxx", "amount": 10}|STEP_1  |
 2|{"code": "45648asd", "step": "STEP_1", "other": null, "amount": 30} |STEP_1  |
 4|{"code": "POIUASDF", "step": "STEP_1", "other": null, "amount": 50} |STEP_1  |
 4|{"code": "34567DTH", "step": "STEP_4", "other": null, "amount": 80} |STEP_4  |
```

