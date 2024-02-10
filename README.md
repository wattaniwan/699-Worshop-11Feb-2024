## Workshop: 11 Feb. 2024 


### 1. Create dataset and table 
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

- Now the table contains 42,731 rows.

### 2. Append data in to table 
- Upload data in **"2confirmed-cases-since-280265-0365part1.csv"** to append with table **data_covid19_raw** 


- Now the table contains 92,730 rows

- [Optional] Command for rename table

```
ALTER TABLE data_covid19_xxx.data_covid19 RENAME TO data_covid19_raw;
```

### 3. Create New Columns and Transform Date and Age in Years 
- Create new columns

```
ALTER TABLE data_covid19_xxx.data_covid19_raw
ADD COLUMN IF NOT EXISTS ANNOUCE_DATE DATE,
ADD COLUMN IF NOT EXISTS NOTIFICATION_DATE DATE,
ADD COLUMN IF NOT EXISTS AGE_YEAR FLOAT64;
```

- Transform Date

```
UPDATE data_covid19_xxx.data_covid19_raw
   SET ANNOUCE_DATE = PARSE_DATE('%d/%m/%Y', Announce_DateStr)
   WHERE Announce_DateStr  IS NOT NULL;

```

```
UPDATE data_covid19_xxx.data_covid19_raw
   SET NOTIFICATION_DATE = PARSE_DATE('%d/%m/%Y', Notify_DateStr)
   WHERE Notify_DateStr  IS NOT NULL;
```

- Transform Age in Years 

      Exploration:

```
SELECT DISTINCT AgeStr
FROM data_covid19_xxx.data_covid19_raw;
```

```
SELECT DISTINCT Unit
FROM data_covid19_xxx.data_covid19_raw;
```

      Transformation:

```
UPDATE data_covid19_xxx.data_covid19_raw
  SET AGE_YEAR = SAFE_CAST(AgeStr AS FLOAT64)
  WHERE AgeStr IS NOT NULL;
```
  
```
UPDATE data_covid19_xxx.data_covid19_raw
SET AGE_YEAR = AGE_YEAR/12
WHERE Unit = "เดือน";

UPDATE data_covid19_xxx.data_covid19_raw
SET AGE_YEAR = AGE_YEAR/365
WHERE Unit = "วัน";
```

### 4. Append data in to table 
- Upload data in **"3confirmed-cases-since-120465.csv"** to append with table **data_covid19_raw** 


- Now the table contains 194,529  rows

### 5. Create Looker Studio 

- Create Covid19 Data Report

### 6. Insert data in table **data_covid19_raw** 

- Insert data

```
INSERT INTO data_covid19_xxx.data_covid19_raw (ID, 
  Announce_DateStr,
  Notify_DateStr, 
  Sex, 
  AgeStr, 
  Unit, 
  Nationality, 
  Province_of_isolation, 
  Risk, 
  Province_of_onset, 
  District_of_onset)
SELECT 
    MAX(ID) + 1,
    '10/2/2024', 
    '9/2/2024', 
    'หญิง', 
    '35', 
    'ปี', 
    'Thailand', 
    'สงขลา', 
    'อื่นๆ', 
    'สงขลา', 
    'หาดใหญ่', 
FROM 
    demo_covid19.dataCovid19_raw;
```


- Now the table contains 194,530 rows.


### 7. Update data in table **data_covid19_raw** 

- Upadate data of ID 3951623:

```
UPDATE data_covid19_xxx.data_covid19_raw
SET Sex = 'ชาย',
    Province_of_isolation = 'ราชบุรี'
WHERE ID = 3951623;
```

      View Result:
```
SELECT ID, Sex, Province_of_isolation
FROM data_covid19_xxx.data_covid19_raw
WHERE ID = 3951623;
```
### 8. Delete in table **data_covid19_raw** 

- Delete data of ID 3951622:

```
DELETE FROM data_covid19_xxx.data_covid19_raw
WHERE ID = 3951622;
```

### 9. Refresh Data in Looker Studio


