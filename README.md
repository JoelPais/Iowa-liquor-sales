# Iowa-liquor-sales
## Analysis of Iowa liquor sales

### Executive Summary

1) When was the earliest and latest date for the purchase of liquor?
```sql
select 
    min(date) as initial_date, 
    max(date) as latest_date 
from 
    `bigquery-public-data.iowa_liquor_sales.sales` 
limit 10
```
Answer:
initial_date: 2012-01-03
latest_date	: 2020-11-30	


2) From how many stores, cities and counties is the liquor order coming from 
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
Answer:
Count_of_stores: 2502
Number_of_cities: 456
Number_of_counties: 104


4) Vendor number and name of the company which had the most and lesdt orders
