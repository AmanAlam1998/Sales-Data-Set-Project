create table retail_sales(
transactions_id int primary key,
sale_date date,
sale_time time,
customer_id int,
gender varchar(20),
age int,
category varchar(20),
quantiy int,
price_per_unit numeric(10,2),
cogs numeric(10,2),
total_sale numeric(10,2)
);
drop table if exists retail_sales;
select * from retail_sales;

select * from retail_sales
order by transactions_id asc limit 10;

select count(*) from retail_sales;

--dealing with null values

select * from retail_sales
where transactions_id is null
or
sale_date is null
or
sale_time is null
or 
customer_id is null
or 
gender is null
or
age is null
or 
category is null
or 
quantiy is null
or 
price_per_unit is null
or 
cogs is null
or 
total_sale is null;

update retail_sales
set
    age= coalesce(age,0),
    category= coalesce(category,'others'),
    quantiy= coalesce(quantiy,0),
    price_per_unit= coalesce(price_per_unit,0),
    cogs= coalesce(cogs,0),
    total_sale= coalesce(total_sale,0);

select * from retail_sales
where total_sale=0;

delete * from retail_sales
where transactions_id is null
or 
sale_date is null
or
sale_time is null
or 
customer_id is null
or 
gender is null
or
age is null
or 
category is null
or 
quantiy is null
or 
price_per_unit is null
or 
cogs is null
or 
total_sale is null;

select count(*) from retail_sales;

--How many sales we have

select count(*) total_sales from retail_sales;

--How many customers we have

select count(distinct(customer_id)) from retail_sales;

-- How many category

select distinct(category) as total_No_of_category from retail_sales;

select count(distinct(category)) from retail_sales;

select * from retail_sales;


select total_sale, sale_date from retail_sales
where sale_date='2022-11-05';

select sum(total_sale) as sum_sale, sale_date from retail_sales
where sale_date='2022-11-05'
group by sale_date;

select total_sal, category,quantiy,sale_date
from retail_sales
where category='Clothing' 
and
quantiy>=3
and
to_char(sale_date,'YYYY-MM')='2022-11'

select Category,sum(total_sale) as Sales, count(quantiy) as Total_order
from retail_sales
group by Category;

select * from retail_sales;

select round(avg(age),2) as Average_Age, category
from retail_sales
where category='Beauty'
group by category

select * from retail_sales
where total_sale>1000;

select count(distinct(transactions_id)) as no_of_transactions,gender,category
from retail_sales
group by gender,category
order by category asc;


select 
year,
month,
avg_sales
from
(
select 
extract(YEAR from sale_date)as year,
extract(MONTH from sale_date) as month,
round(avg(total_sale),2) as avg_sales,
rank() over(partition by extract(YEAR from sale_date) order by avg(total_sale) desc) as rank
from retail_sales
group by year,month
) as t1
where rank=1;
--order by year asc, avg_sales desc;

select * from retail_sales;

select customer_id,sum(total_sale)as Sales_Customer
from retail_sales
group by customer_id
order by Sales_Customer desc
limit 5;

select count(distinct(customer_id))as Unigue_Customers,category
from retail_sales
group by category;

with hourly_sales
as(
select *,
case
    when extract(hour from sale_time)<12 then 'Morning'
	when extract(hour from sale_time) between 12 and 17 then 'Afternoon'
	else 'Evening'
end as shift
from retail_sales
)
select
shift,
count(*) as total_orders
from hourly_sales
group by shift;
