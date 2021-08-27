# Iowa-liquor-sales
## Analysis of liquor sales in the state of Iowa

### Executive Summary

#### Q1) When was the earliest and latest date for the purchase of liquor?
```sql
select 
    min(date) as initial_date, 
    max(date) as latest_date 
from 
    `bigquery-public-data.iowa_liquor_sales.sales` 
limit 10
```
#### Answer:
initial_date: 2012-01-03
latest_date	: 2020-11-30	


#### Q2) From how many stores, cities and counties is the liquor order coming from 
```sql
select 
    count(distinct(store_number)) as Count_of_stores,
    count(distinct(lower(city))) as Number_of_cities,
    count(distinct(lower(county))) as Number_of_counties
from 
    `bigquery-public-data.iowa_liquor_sales.sales`
where 
    store_number is not null AND
    city is not null AND 
    county is not null
```
#### Answer:
Count_of_stores: 2502
Number_of_cities: 456
Number_of_counties: 104


#### Q3) Number of stores in each county
```sql
select 
    county_number, lower(county) as county, count(distinct(store_number)) as number_of_stores 
from 
    `bigquery-public-data.iowa_liquor_sales.sales` 
where 
    county_number is NOT Null and
    store_number is NOT Null
group by 
    county_number, county 
order by 
    number_of_stores desc
```

#### Answer:
![image](https://user-images.githubusercontent.com/87647771/130948547-ed47267f-b885-40de-8515-d83b1185bf93.png)


#### Q4) To determine which store has ordered the highest number of liquor bottles 
```sql
# Using CTE
with max_value as 
(select
    max(bottles_sold) as bottles_sold
from 
    `bigquery-public-data.iowa_liquor_sales.sales`)

# Using Inner Join
select 
    date, store_number, store_name, address, city, zip_code, county_number, county, 
    category_name, vendor_name, item_description, a.bottles_sold as bottles_ordered
from 
    `bigquery-public-data.iowa_liquor_sales.sales` b 
inner join 
    max_value a 
on 
    a.bottles_sold = b.bottles_sold 
```

#### Answer:
![image](https://user-images.githubusercontent.com/87647771/130853552-64297824-9fcf-4251-b241-0d975a8c48e5.png)


#### To determine which store has ordered the least number of liquor bottles 
```sql
# Using Sub-query
SELECT
  store_number, store_name, address, city, zip_code, county,
  category_name, vendor_name, item_description, bottles_sold AS bottles_ordered
FROM
  `bigquery-public-data.iowa_liquor_sales.sales`
WHERE
  bottles_sold IN (
  SELECT
    bottles_sold
  FROM
    `bigquery-public-data.iowa_liquor_sales.sales`
  ORDER BY
    bottles_sold
  LIMIT
    10)
ORDER BY
  store_number
```

#### Answer:
![image](https://user-images.githubusercontent.com/87647771/130854653-d7f56f07-564c-487d-9c5f-eab5d611ee2a.png)


#### Q5) Which county has spent the most for liquor
```sql
# Using CTE
WITH
  highest_cost AS (
  SELECT
    sale_dollars
  FROM
    `bigquery-public-data.iowa_liquor_sales.sales`
  ORDER BY
    sale_dollars DESC
  LIMIT
    50) 

# Using Inner Join
SELECT
  a.county_number, LOWER(a.county) AS county, SUM(b.sale_dollars) AS highest_cost
FROM
  `bigquery-public-data.iowa_liquor_sales.sales` a
INNER JOIN
  highest_cost b
ON
  b.sale_dollars = a.sale_dollars
GROUP BY
  a.county_number, county
ORDER BY
  highest_cost DESC
```

#### Answer:
![image](https://user-images.githubusercontent.com/87647771/130987251-40784cea-eaf5-45b3-b402-b16189a4b81c.png)
