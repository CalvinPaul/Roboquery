# Become a Google Bigquery Expert. Free PDF Cheatsheet with 30+ Bigquery SQL snippets

[Download the free pdf with Bigquery code snippets](https://github.com/CalvinPaul/Roboquery/blob/master/pages/Google_Bigquery_Expert_Cheatsheet.pdf?raw=true)

All these snippets are also included in the free Roboquery chrome extension 


## Create Table:
```sql
CREATE TABLE Dataset.TableName
(
   EmployeeNo INT64 NOT NULL,
   FirstName STRING,
   LastName STRING,
   DOB DATE
   )
PARTITION BY DOB   
CLUSTER BY EmployeeNo;
```

## Create View:
```sql
CREATE OR REPLACE VIEW Dataset.ViewName AS
SELECT 
      EmployeeNo,
      FirstName,
      LastName,
      DOB,
      JoinedDate,
      DepartmentNo
FROM Dataset.TableName;
```

## Create table as select:

```sql
CREATE TABLE mydataset.mynewtable
 AS 
 SELECT * FROM mydataset.myothertable;
```

## Drop Table:
```sql
DROP TABLE Dataset.Tablename;
```

## Drop View:
```sql
DROP VIEW Dataset.Viewname;
```

## Create CTE:
```sql
WITH subQ1 AS (SELECT SchoolID FROM Roster),
subQ2 AS (SELECT OpponentID FROM PlayerStats)
SELECT * FROM subQ1
UNION ALL
SELECT * FROM subQ2;
```


## Derived Table:
```sql
SELECT r.LastName FROM
( SELECT * FROM Roster) AS r;
```

## Select Distinct:
```sql
SELECT DISTINCT * FROM DatasetName.MyTable;
```

## Select using Timetravel:
```sql
SELECT * FROM Dataset.Table
FOR SYSTEM_TIME AS OF '2019-01-01 10:00:00-07:00';
```

## Group By:
```sql
SELECT LastName, SUM(PointsScored) as pts
FROM PlayerStats
GROUP BY LastName;
```

## Group by Having:
```sql
SELECT LastName, SUM(PointsScored) AS ps
FROM Roster
GROUP BY LastName
HAVING ps > 0;
```

## Order By:
```sql
SELECT LastName, PointsScored, OpponentID
FROM PlayerStats
ORDER BY SchoolID, LastName desc;
```

## Insert into Table:
```sql
INSERT into dataset.Inventory_New 
(
  product, 
  quantity, 
  supply_constrained
)
SELECT * FROM dataset.Inventory;
```

## Insert values into Table:

INSERT into dataset.Inventory (product, quantity)
VALUES('top load washer', 10),
      ('front load washer', 20),
      ('dryer', 30),
      ('refrigerator', 10);


Update Table:
UPDATE dataset.Inventory
SET quantity = quantity - 10
WHERE product like '%washer%';


Update From:
UPDATE dataset.Inventory i
SET quantity = n.quantity
FROM (
select quantity,product from dataset.NewArrivals
)n
WHERE i.product = n.product;

Merge:
MERGE dataset.Inventory T
USING dataset.NewArrivals S
ON T.product = S.product
WHEN MATCHED THEN
  UPDATE SET quantity = T.quantity + S.quantity
WHEN NOT MATCHED THEN
  INSERT (product, quantity) VALUES(product, quantity);

Delete:

DELETE FROM dataset.Inventory WHERE quantity = 0;
--to delete all rows
DELETE FROM dataset.DetailedInventory WHERE true;

Inner Join:
SELECT R.* FROM Roster R
INNER JOIN PlayerStats P
ON R.LastName = P.LastName;


Left Join:
SELECT R.* FROM Roster R
LEFT JOIN PlayerStats P
ON R.LastName = P.LastName;

Parse Date:

SELECT PARSE_DATE("%Y%m%d", "20190927") as parsed_date;

Date Add:

SELECT DATE_ADD(DATE "2008-12-25", INTERVAL 5 DAY) as five_days_later;

Date Sub:

SELECT DATE_SUB(DATE "2008-12-25", INTERVAL 5 DAY) as five_days_ago;



Date Diff:

SELECT DATE_DIFF(DATE '2010-07-07', DATE '2008-12-25', DAY) as days_diff;

Extract from Date:

SELECT EXTRACT(DAY FROM DATE '2013-12-25') as the_day;


Cast to Integer:

SELECT CAST("123" AS INT64) AS Emp_Id;

Cast to String:
SELECT CAST(123 AS STRING) AS Emp_Id;

Cast to Numeric:

SELECT CAST("123" AS NUMERIC) AS Emp_Id;





Rank Over:

SELECT firstname, department, startdate,
RANK() OVER ( PARTITION BY department ORDER BY startdate ) AS rank
FROM Employees;

Row_Number:

SELECT firstname, department, startdate,
ROW_NUMBER() OVER ( PARTITION BY department ORDER BY startdate ) AS rank
FROM Employees;

Rows Between:
select firstname,
SUM(x) OVER (PARTITION BY y ORDER BY z ROWS BETWEEN 1 PRECEDING AND 1
FOLLOWING) as ColumnAlias
from Employee;


This free cheat sheet is brought to you by Roboquery. Roboquery helps you convert your table DDL, views, SQL scripts to Google Bigquery. Install the free chrome extension now
