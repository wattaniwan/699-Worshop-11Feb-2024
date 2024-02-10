## Workshop: 11 Feb. 2024 


### Create dataset and table 
- Create dataset **data_covid19_xxx** in BigQuery via WebUI 

- Create table **data_covid19_raw** via WebUI upload data in  **"1confirmed-cases-since-280265-0265.csv"** with non-auto detect and set Field names as:

```
ID, 
Announce_DateStr,
Notify_DateStr, 
Sex, 
AgeStr, 
Unit, 
Nationality, 
Province_of_isolation, 
Risk, 
Province_of_onset, 
District_of_onset
```


- Set **Header rows to skip = 1**

- Now the table contains 42731 rows.

## Append data in to table 
- Upload data in **"2confirmed-cases-since-280265-0365part1.csv"** to append with table **data_covid19_raw** 


- Now the table contains 92730 rows

- [Optional] Command for rename table

```
ALTER TABLE demo_covid19_xxx.data_covid19 RENAME TO data_covid19_raw;
```

### Create New Columns and Transform Date and Age in Years 
- Create new columns

```
ALTER TABLE demo_covid19_xxx.data_covid19_raw
ADD COLUMN IF NOT EXISTS ANNOUCE_DATE DATE,
ADD COLUMN IF NOT EXISTS NOTIFICATION_DATE DATE,
ADD COLUMN IF NOT EXISTS AGE_YEAR FLOAT64;
```

- Transform Date

```
UPDATE demo_covid19_xxx.data_covid19_raw
   SET ANNOUCE_DATE = PARSE_DATE('%d/%m/%Y', Announce_DateStr)
   WHERE Announce_DateStr  IS NOT NULL;

```

```
UPDATE demo_covid19_xxx.data_covid19_raw
   SET NOTIFICATION_DATE = PARSE_DATE('%d/%m/%Y', Notify_DateStr)
   WHERE Notify_DateStr  IS NOT NULL;
```

- Transform Age in Years 

$\ \ \ \ \ \ $   Exploration:

```
SELECT DISTINCT AgeStr
FROM demo_covid19_xxx.data_covid19_raw;
```

```
SELECT DISTINCT Unit
FROM demo_covid19_xxx.data_covid19_raw;
```

$\ \ \ \ \ \ $   Transformation:

```
UPDATE demo_covid19_xxx.data_covid19_raw
SET AGE_YEAR = AGE_YEAR/12
WHERE Unit = "เดือน";

UPDATE demo_covid19_xxx.data_covid19_raw
SET AGE_YEAR = AGE_YEAR/365
WHERE Unit = "วัน";
```

## Append data in to table 
- Upload data in **"3confirmed-cases-since-120465.csv"** to append with table **data_covid19_raw** 


- Now the table contains xxx  rows


