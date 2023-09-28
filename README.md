# Sales_data_analysis_SQL_Excel
Analyzing sales data using Postgresql and Microsoft Excel. Dataset was acquired from Kaggle.

The sql queries used are as follows:

create table sales_data(S_no int, order_id int, product varchar, Quantity_Ordered int, price_Each decimal(10, 2), Order_Date date, Purchase_Address varchar, Month int, Sales decimal(10,2), City varchar, Hour int );

select * from sales_data;

copy sales_data from 'E:\postgre\Sales Data.csv' delimiter ',' csv header;

/*Top 10 most selling products*/
select distinct product, sum(sales) as total_sales from sales_data group by product order by total_sales desc limit 10;

/*Top cities by total sales*/
select distinct City, sum(sales) as total_sales from sales_data group by City order by total_sales desc;

/*Total Sales by Months*/
select distinct month, sum(sales) as total_sales from sales_data group by month order by total_sales desc; 

/*days most sales were made*/
select distinct order_date, sum(sales) as total_sales from sales_data group by order_date order by total_sales desc limit 10; 

/*Top 10 addresses by sales. Can be interpreted as top 10 customers*/
select distinct purchase_address, sum(sales) as total_sales from sales_data group by purchase_address order by total_sales desc limit 10; 

/*Which hours of the day most sales take place*/
select distinct hour, sum(sales) as total_sales from sales_data group by hour order by total_sales desc limit 10; 

/*Top 10 products by quantites ordered*/
select distinct product, sum(quantity_ordered) as total_quantity from sales_data group by product order by total_quantity desc limit 10; 

/*Adding a column that shows which half of the year the sale took place*/
ALTER TABLE sales_data
ADD COLUMN which_half VARCHAR;

UPDATE sales_data
SET which_half = CASE
    WHEN month BETWEEN 1 AND 6 THEN 'First Half'
    WHEN month BETWEEN 7 AND 12 THEN 'Second Half'
    ELSE 'Invalid Month'
END;

/*Which half of the year is more profitable?*/
select which_half, sum(sales) as total_sales from sales_data group by which_half order by total_sales desc;
