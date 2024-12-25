* Fetch the employee number, first name and last name of those employees who are working as Sales Rep reporting to employee with employeenumber 1102 (Refer employee table)

```yaml
select employeenumber,firstname,lastname from employees
where reportsTo = 1102;
```

* 	Show the unique productline values containing the word cars at the end from the products table.
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

 
```yaml


```
