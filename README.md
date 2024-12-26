1. Fetch the employee number, first name and last name of those employees who are working as Sales Rep reporting to employee with employeenumber 1102 (Refer employee table)

```yaml
select employeenumber,firstname,lastname from employees
where reportsTo = 1102;
```

![output](https://github.com/Mujahid-max/Mysql_project/blob/0b6e25c2de7a54a19389d7738255fad28b039c3c/Picture1.png)


2. Show the unique productline values containing the word cars at the end from the products table.
Expected output:

```yaml
select distinct productline from products where productLine like "% Cars";
```
![output](https://github.com/Mujahid-max/Mysql_project/blob/e0b93e261a354cac2dfb80138fd4573a3b0aa0d9/Picture2.png)

3. Show the unique productline values containing the word cars at the end from the products table.
Expected output:

```yaml
select customernumber,customername,
case 
when country in ("Canada","USA") then "North America"
    when country  in ("UK","France","Germany") then "Europe"
else "other"
 end
customersegment
from customers;

```
![output](https://github.com/Mujahid-max/Mysql_project/blob/86d9ef9234e42dff98360737a8e4ad249bf2339c/Picture3.png)


4. Using the OrderDetails table, identify the top 10 products (by productCode) with the highest total order quantity across all orders.

```yaml
SELECT 
    productcode, SUM(quantityordered) AS totalordered
FROM
    orderdetails
GROUP BY productcode
ORDER BY totalordered DESC
LIMIT 10;
```
![output](https://github.com/Mujahid-max/Mysql_project/blob/ed0e642e9aa9f42e633cb3c5b2ed7bcaf5c662bb/Picture4.png)

5. Company wants to analyse payment frequency by month. Extract the month name from the payment date to count the total number of payments for each month and include only those months with a payment count exceeding 20. Sort the results by total number of payments in descending order. 

```yaml
SELECT 
    MONTHNAME(paymentDate) AS payment_month,
    COUNT(paymentdate) AS num_payment
FROM
    payments
GROUP BY MONTHNAME(paymentDate) , payment_month
HAVING num_payment > 20;

```
![output](https://github.com/Mujahid-max/Mysql_project/blob/61b081e2a4564834c10ffb05c2123b4ae604fc60/Picture5.png)

6. customer_id: This should be an integer set as the PRIMARY KEY and AUTO_INCREMENT.
* first_name: This should be a VARCHAR(50) to store the customer's first name.
* last_name: This should be a VARCHAR(50) to store the customer's last name.
* email: This should be a VARCHAR(255) set as UNIQUE to ensure no duplicate email addresses exist.
* phone_number: This can be a VARCHAR(20) to allow for different phone number formats.

#### Add a NOT NULL constraint to the first_name and last_name columns to ensure they always have a value.

```yaml
create table customers(
customer_id int primary key auto_increment,
first_name Varchar(50),
last_name Varchar(50),
email varchar(255),
phone_number varchar(20)
);
alter table customers modify  first_name varchar(50)  not null;

alter table customers modify last_name varchar(50)  not null;

desc customers;
```

7. order_id: This should be an integer set as the PRIMARY KEY and AUTO_INCREMENT.
* customer_id: This should be an integer referencing the customer_id in the Customers table  (FOREIGN KEY).
* order_date: This should be a DATE data type to store the order date.
* total_amount: This should be a DECIMAL(10,2) to store the total order amount.
     	
#### Constraints:
* Set a FOREIGN KEY constraint on customer_id to reference the Customers table.
* Add a CHECK constraint to ensure the total_amount is always a positive value.

```yaml
create table orders(
order_id int primary key not null auto_increment,
customer_id int not null,
order_date date,
total_amount decimal(10,2) NOT NULL CHECK (total_amount >0),
constraint fk_customer foreign key(customer_id)references customers(customer_id)
);

```


8. List the top 5 countries (by order count) that Classic Models ships to. (Use the Customers and Orders tables)

```yaml
select customers.country,count(orders.ordernumber)as order_count from orders
join customers on customers.customerNumber = orders.customerNumber
group by customers.country
order by order_count desc
limit 5;
```
![output](https://github.com/Mujahid-max/Mysql_project/blob/be9fd5efef8b7a15f224d630a64a8c1c9baa700b/Picture6.png)

9. DDL Commands: Create, Alter, Rename
* Create table facility. Add the below fields into it.
* Facility_ID
* Name
* State
* Country

* Alter the table by adding the primary key and auto increment to Facility_ID column.
* Add a new column city after name with data type as varchar which should not accept any null values.

```yaml
create table facility(
Facility_id int,
Name varchar(100),
State varchar(100),
Country Varchar(100)
);

alter table facility modify facility_id int primary key auto_increment;
desc facility;
alter table facility add column city varchar(100) not null after name;
```
![output](https://github.com/Mujahid-max/Mysql_project/blob/746a82c89326f25b2bce6d6a277ac4cf5a46aad4/Picture7.png)

10. Create a view named product_category_sales that provides insights into sales performance by product category. This view should include the following information:
productLine: The category name of the product (from the ProductLines table).

* total_sales: The total revenue generated by products within that category (calculated by summing the orderDetails.quantity * orderDetails.priceEach for each product in the category).

* number_of_orders: The total number of orders containing products from that category.

```yaml
create view product_category_sales as
SELECT 
productlines.productline AS productline,
sum(orderdetails.quantityordered * orderdetails.priceEach) AS total_sales,
count(distinct orders.ordernumber) AS number_of_orders
FROM 
productlines
JOIN 
products ON products.productline = productlines.productline
JOIN 
orderdetails ON products.productcode = orderdetails.productcode
JOIN 
orders ON orders.ordernumber = orderdetails.ordernumber
group by productLine;

```
![output](https://github.com/Mujahid-max/Mysql_project/blob/88a33b0f4b837403fc4594082eca49909a1b5e0a/Picture8.png)

11. Using customers and orders tables, rank the customers based on their order frequency

```yaml
with customerordercounts as (
select
c.customername,count(o.ordernumber) as order_count
from
customers c
join
orders o on c.customerNumber = o.customerNumber
group by
c.customerName
),
rankedcustomers as (
select 
customername,
order_count,
rank() over (order by order_count desc) as order_frequency_rank,
dense_rank() over	(order by Order_count desc) as order_frequency_Dense_rank
from
customerordercounts
)
select 
customername,
order_count,
order_frequency_rank as order_frequency_mk
from 
rankedcustomers
order by 
order_frequency_rank;

```
![output](https://github.com/Mujahid-max/practice/blob/main/Picture9.png?raw=true)


12. Calculate year wise, month name wise count of orders and year over year (YoY) percentage change. Format the YoY values in no decimals and show in % sign.

```yaml
with growth as (
select year(orderdate) as `order year`,
monthname(orderdate) as `order month`,
count(orderdate) as `total orders`
from orders
group by
`order year`,`order month`
)
select 		
`order year`,
    `order month`,
    `total orders`,
    concat(round(100* ((`total orders`-lag(`total orders`)over(order by `order year`))/lag(`total orders`) over(order by `order year`)
),
0
),'%'
)as "% YOY Changes"
from growth;            
```
![output](https://github.com/Mujahid-max/practice/blob/main/Picture10.png?raw=true)

