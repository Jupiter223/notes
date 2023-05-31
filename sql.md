# SQL

point

## is Null/!=

```sql
select name from Customer
where referee_id!=2 or referee_id is null;
```

```sql
Select name from Customer where referee_id is null or referee_id <> 2
```

## not in

```sql
SELECT name as Customers FROM Customers
where id NOT IN 
(Select customerId FROM Orders);
```

## if条件式 like

```sql
select employee_id,if(employee_id%2=1 and name not like 'M%',salary,0 ) as bonus
from Employees
```



```sql
select stock_name,sum(if(operation='Sell',price,-price)) as capital_gain_loss
from Stocks
group by stock_name
```



## case

```sql
update salary
set sex = 
case sex
when 'm' then 'f'
else 'm'
end;
```

```sql
select id,(
case 
when p_id is null then 'Root'
when id in (select distinct p_id from tree) then 'Inner'
else 'Leaf'
end
) as type
from Tree
```

endを忘れないで

## delete  

```sql
delete p1 from Person p1,Person p2
where p1.email=p2.email and p1.id>p2.id
```

## left(column_name,1)

```sql
select user_id,(CONCAT(UPPER(LEFT(name,1)),LOWER(SUBSTRING(name,2))))as name
from Users
order by user_id
```

## GROUP BY, GROUP_CONCAT, COUNT()

```SQL
SELECT
    sell_date,
    COUNT(DISTINCT product) AS num_sold,
    GROUP_CONCAT(DISTINCT product SEPARATOR ',') AS products
FROM
    Activities
GROUP BY
    sell_date
ORDER BY
    sell_date, product;

```

```sql
# group by 2 column
select  date_id,make_name,count(distinct lead_id) as unique_leads,count(distinct partner_id) as unique_partners
from DailySales
group by date_id, make_name
```



## REGEXP

```sql
SELECT * FROM patients WHERE conditions REGEXP '\\bDIAB1'
```

## not in /union

```sql
select employee_id 
from Employees
where employee_id not in (select employee_id from Salaries)
union
select employee_id
from Salaries
where employee_id not in (select employee_id from Employees)
order by employee_id
```

max()

```sql
select max(salary) as SecondHighestSalary
from Employee
where salary not in (select max(salary) from Employee)
```

## Join

```sql
select p.firstName , p.lastName, a.city , a.state 
    from Person p LEFT JOIN Address a on p.personId  = a.personId  
```

```sql
select v.customer_id,count(v.customer_id) as count_no_trans
from Visits v 
Left join Transactions t
on v.visit_id =t.visit_id
where t.transaction_id is null

group by v.customer_id
```

```sql
select name
from SalesPerson
where sales_id not in(

select sales_id 
from Orders o
left join Company c
on o.com_id=c.com_id
where name='RED')
```



## distinct

```sql
# Write your MySQL query statement below

select distinct viewer_id as id
from Views
where viewer_id=author_id
order by viewer_id asc
```

## datediff

```sql
select w1.id 
from Weather w1,Weather w2
where w1.temperature>w2.temperature
and datediff(w1.recordDate,w2.recordDate)=1
```

```
SELECT DATEDIFF('2008-11-30','2008-11-29') AS DiffDate
```

## with e as ()  order by

```sql
with e as (select customer_number,count(order_number) as count
from Orders
group by customer_number)

select customer_number
from e
order by e.count desc
limit 1
```

## min(date) max(date)

```sql
# Write your MySQL query statement below
select player_id,min(event_date) as first_login
from Activity
group by player_id


```

```sql
select user_id,max(time_stamp) as last_stamp
from Logins
where time_stamp>='2020-01-01 00:00:00' and time_stamp<'2021-01-01 00:00:00'
group by user_id
```

## sum

```
select event_day as day,emp_id,sum(out_time-in_time) as total_time
from Employees
group by event_day,emp_id
```

```sql
select u.name,sum(if(r.distance is not null,r.distance,'0')) as travelled_distance
from Users u left join Rides r on u.id=r.user_id
group by u.id
order by travelled_distance desc,u.name
```

### YEAR(2019)

```sql
select u.user_id as buyer_id,u.join_date,count(o.order_id) as orders_in_2019
from Users u left join Orders o on u.user_id=o.buyer_id
and YEAR(order_date)='2019'
group by u.user_id
```

## Having

```sql
select actor_id,director_id from ActorDirector group by actor_id,director_id having count(*)>=3;
```

The `HAVING` clause is used in SQL queries to filter the result set based on conditions that involve aggregated values. It is similar to the `WHERE` clause, but while the `WHERE` clause filters individual rows before grouping and aggregating, the `HAVING` clause filters groups after grouping and aggregating.

```sql
# Write your MySQL query statement below
select u.name,sum(t.amount) as balance
from Users u join Transactions t on
u.account=t.account
group by u.account
having balance>'10000'
```

## inner join

```sql
select product_id,product_name from Sales
inner join product using(product_id)  
group by product_id
having sum(if(sale_date between '2019-01-01' and '2019-03-31', 1, 0)) = sum(if(sale_date, 1, 0))
```

## DATE_TRUNC('datepart',timestamp)