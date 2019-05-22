Q2. Create an employee table in the metastore that contains the employee records stored in HDFS.

sqoop import --connect jdbc:mysql://172.0.0.1:3306/loudacre --username training --password training --table employee --target-dir /user/training/problem2/data/employee/ --as-parquetfile

sqoop create-hive-table --connect jdbc:mysql://127.0.0.1:3306/loudacre --username training --password training --table employee --hive-table solution


Q3. Generate a table that contains all customers who have negative account balances

select c.id, c.fname, c.lname, c.hphone
from account a
join customer c on (a.custid = c.id)
where a.amount < 0;
limit 10;

Q5. The bank is making a Facebook group for the Palo Alto, CA branch. Generate a script that outputs the customers and employees who live in Palo Alto, CA.

select c.fname, c.lname, c.city, c.state
from customer c
where c.state = 'CA'
and c.city = 'Palo Alto'
union all
select e.fname, e.lname, e.city, e.state
from employee e 
where e.state = 'CA'
and e.city = 'Palo Alto';

Q6. There are privacy concerns about the employee data that is stored on the cluster. Your task is to remove any age information from the employee data by creating a new table for the data analysts
to query against.


CREATE TABLE solution AS
SELECT id, fname, lname, address, city, state, zip, 
substr(birthday, 1,5) birthday
FROM employee;

Q7. Generate a report that contains all of the Seattle employee names in sorted order.

select concat(e.fname, ' ', e.lname) as name
from employee e
where e.city = 'Seattle'
sort by name;

Q8. Use Sqoop to export customer data from HDFS into a MySQL database table. Place the data in the solution table in MySQL, which has been created and is currently empty.

sqoop export --connect jdbc:mysql://127.0.0.1/problem8 --username cloudera --password cloudera --table solution --export-dir /user/training/problem8/data/customer/


Q9. Your company is being acquired by another company. To prepare for this acquisition, update the customer records to guarantee there will be no duplicate IDs with their existing customer IDs.

CREATE TABLE solution AS
SELECT concat('A', '', id) as newId, fname, lname, address, city, state, zip, birthday
FROM customer;


Q10. Your boss needs specialized reports using the billing data and is constantly asking for help to write SQL queries. Create a database view in the metastore so that your boss has customer and billing data joined.

create view solution as 
select b.id as id, c.fname as fname,
c.lname as lname, c.city as city, c.state as state,
b.charge as charge, b.tstamp as billdata
from billing b
join customer c on (b.id = c.id);

Q11. Several analysis questions are described below and you will need to write the SQL code to answer them. You can use whichever tool you prefer? Impala or Hive ? using whichever method you like best, including shell, script, or the Hue Query Editor, to run your queries.

select count(o.order_id) as cnt, prod_id
from order_details o
group by o.prod_id
order by cnt desc;


