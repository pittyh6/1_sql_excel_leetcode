### Table
Insurance

| Column Name | Type  |
|:------------|:------|
| pid         | int   |
| tiv_2015    | float |
| tiv_2016    | float |
| lat         | float |
| lon         | float |

| pid | tiv_2015 | tiv_2016 | lat  | lon  |
| --- | -------- | -------- | ---- | ---- |
| 1   | 224.17   | 952.73   | 32.4 | 20.2 |
| 2   | 224.17   | 900.66   | 52.4 | 32.7 |
| 3   | 824.61   | 645.13   | 72.4 | 45.2 |
| 4   | 424.32   | 323.66   | 12.4 | 7.7  |
| 5   | 424.32   | 282.9    | 12.4 | 7.7  |
| 6   | 625.05   | 243.53   | 52.5 | 32.8 |
| 7   | 424.32   | 968.94   | 72.5 | 45.3 |
| 8   | 624.46   | 714.13   | 12.5 | 7.8  |
| 9   | 425.49   | 463.85   | 32.5 | 20.3 |
| 10  | 624.46   | 776.85   | 12.4 | 7.7  |
| 11  | 624.46   | 692.71   | 72.5 | 45.3 |
| 12  | 225.93   | 933      | 12.5 | 7.8  |
| 13  | 824.61   | 786.86   | 32.6 | 20.3 |
| 14  | 824.61   | 935.34   | 52.6 | 32.8 |

<hr>

### Problem: 585. Investments in 2026
pid is the primary key (column with unique values) for this table.
Each row of this table contains information about one policy where:
pid is the policyholder's policy ID.
tiv_2015 is the total investment value in 2015 and tiv_2016 is the total investment value in 2016.
lat is the latitude of the policy holder's city. It's guaranteed that lat is not NULL.
lon is the longitude of the policy holder's city. It's guaranteed that lon is not NULL.
 

Write a solution to report the sum of all total investment values in 2016 tiv_2016, for all policyholders who:

have the same tiv_2015 value as one or more other policyholders, and
are not located in the same city as any other policyholder (i.e., the (lat, lon) attribute pairs must be unique).
Round tiv_2016 to two decimal places.

<hr>

### SQL Solution
1) First solution
```sql
WITH get_unique_location AS (
    SELECT pid, tiv_2015, tiv_2016 , lat, lon
    FROM Insurance
    GROUP BY lat, lon
    HAVING COUNT(*) = 1
), get_mult_tiv_2015 AS (
    SELECT pid, tiv_2015 
    FROM Insurance
    GROUP BY tiv_2015
    HAVING COUNT(*) > 1
)

SELECT ROUND(SUM(gul.tiv_2016),2) AS tiv_2016
FROM get_mult_tiv_2015 gmt 
INNER JOIN get_unique_location gul 
    ON gmt.tiv_2015 = gul.tiv_2015
```
2) Second Solution
```sql
--Runtime: 549ms 
WITH get_partition AS (
    SELECT 
        tiv_2016,
        COUNT(*) OVER (PARTITION BY lat, lon) AS get_location,
        COUNT(tiv_2015) OVER (PARTITION BY tiv_2015) AS get_tiv_2015
    FROM Insurance
)
SELECT ROUND(SUM(tiv_2016),2) AS tiv_2016
FROM get_partition 
WHERE get_location = 1 AND get_tiv_2015 > 1
```

<hr>

### Excel Solution


