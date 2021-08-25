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


3) To determine which store ordered the highest number of liquor bottles along with its vendor
```sql
with max_value as 
(select
    max(bottles_sold) as bottles_sold
from 
    `bigquery-public-data.iowa_liquor_sales.sales`)

select 
    date, store_number, store_name, address, city, zip_code, county_number, county, category_name, vendor_name, item_description, a.bottles_sold as bottles_ordered
from 
    `bigquery-public-data.iowa_liquor_sales.sales` b 
inner join 
    max_value a 
on 
    a.bottles_sold = b.bottles_sold 
```

Answer:
![image](https://user-images.githubusercontent.com/87647771/130853552-64297824-9fcf-4251-b241-0d975a8c48e5.png)


To determine which store ordered the least number of liquor bottles along with its vendor
```sql
with min_value as 
(select
    min(bottles_sold) as bottles_sold
from 
    `bigquery-public-data.iowa_liquor_sales.sales`)

select 
    store_number, store_name, address, city, zip_code, county, category_name, vendor_name, item_description, h.bottles_sold as bottles_ordered
from 
    `bigquery-public-data.iowa_liquor_sales.sales` g
inner join 
    min_value h 
on 
    g.bottles_sold = h.bottles_sold 
order by 
    store_number
```

Answer:
![image](https://user-images.githubusercontent.com/87647771/130854653-d7f56f07-564c-487d-9c5f-eab5d611ee2a.png)


4) 

