# DatabaseAssignment4_security

Excercise 1 

User privileges to inventory, sales, accountant, products, and IT for Database classicmodels  

Database changed
mysql> show tables;
+-------------------------+
| Tables_in_classicmodels |
+-------------------------+
| customers               |
| employees               |
| offices                 |
| orderdetails            |
| orders                  |
| payments                |
| productlines            |
| products                |
+-------------------------+
8 rows in set (0.00 sec)


In order to enable logging to query the user must type:
SET global general_log = 1;
SET global log_output = 'table';


Inventory:  involves the tables products and productlines. These should be granted the privileges for read/write/delete permissions
    CREATE USER 'Inventory'@'%' IDENTIFIED BY 'password';
    GRANT SELECT,INSERT,UPDATE,DELETE  ON classicmodels.products TO 'Inventory'@'%';
    GRANT SELECT,INSERT,UPDATE,DELETE  ON classicmodels.productlines TO 'Inventory'@'%';
    FLUSH PRIVILEGES; 

Bookkeeping: involves tables customers, orders, payments, employees and offices. Privilege granted for all the table is read/write. 
    CREATE USER 'Bookkeeping'@'%' IDENTIFIED BY 'password';
    GRANT SELECT, INSERT, UPDATE ON classicmodels . customers TO 'Bookkeeping'@'%';
    GRANT SELECT, INSERT, UPDATE ON classicmodels . payments TO 'Bookkeeping'@'%';
    GRANT SELECT ON classicmodels . orderdetails TO 'Bookkeeping'@'%';
    GRANT SELECT ON classicmodels . orders TO 'Bookkeeping'@'%';
    GRANT SELECT ON classicmodels . productlines TO 'Bookkeeping'@'%';
    GRANT SELECT ON classicmodels . products TO 'Bookkeeping'@'%';
    FLUSH PRIVILEGES;

Human ressources: involves tables employee and offices. Privileges is read/write/delete as sometimes the rows needs to be updated and deleted if a employee has moved or quited.
    CREATE USER 'humanresource'@'%' IDENTIFIED BY 'password';
    GRANT SELECT, INSERT, UPDATE, DELETE ON classicmodels.employees TO 'humanresource'@'%';
    GRANT SELECT, INSERT, UPDATE, DELETE ON classicmodels.offices TO 'humanresource'@'%';
    FLUSH PRIVILEGES;  


Sales: involves tables orders, order details and customer. Privileges are read/write/delete.  
    CREATE USER 'Sales'@'%' IDENTIFIED BY 'password';
    GRANT SELECT, INSERT, UPDATE ON classicmodels . orders TO 'Sales'@'%';
    GRANT SELECT, INSERT, UPDATE ON classicmodels . orderdetails TO 'Sales'@'%';
    GRANT SELECT, INSERT, UPDATE ON classicmodels . customers TO 'Sales'@'%';
    FLUSH PRIVILEGES;

IT: All privilegies  - måske skrive noget tekst om de forskellige roller der findes i IT afdelingen.
    CREATE USER 'IT'@'localhost' IDENTIFIED BY 'password';
    GRANT ALL PRIVILEGES ON classicmodels . * TO 'IT'@'localhost';
    FLUSH PRIVILEGES;

Excercise 2

mysql> select * from employees;
+----------------+-----------+-----------+-----------+---------------------------------+------------+-----------+----------------------+
| employeeNumber | lastName  | firstName | extension | email                           | officeCode | reportsTo | jobTitle             |
+----------------+-----------+-----------+-----------+---------------------------------+------------+-----------+----------------------+
|           1002 | Murphy    | Diane     | x5800     | dmurphy@classicmodelcars.com    | 1          |      NULL | President            |
|           1056 | Patterson | Mary      | x4611     | mpatterso@classicmodelcars.com  | 1          |      1002 | VP Sales             |
|           1076 | Firrelli  | Jeff      | x9273     | jfirrelli@classicmodelcar


Employee
INSERT INTO classicmodels.employees (employeeNumber,lastname, firstname, extension, email, officeCode, reportsTo, jobTitle) values (1703, 'Jensen','Gert','x1000','gj@hotmail.com','1','1002','Sales');

INSERT INTO classicmodels.employees(employeeNumber,lastName,firstName,extension,email,officeCode,reportsTo,jobTitle)VALUES(8,'Knudsen','Peter','x2000','prod@live.dk',1,1002,'production');


Product
INSERT INTO classicmodels.products(productCode,productName,productLine,productScale, productVendor, productDescription,quantityInStock,buyPrice,MSRP) 
 VALUES ('S76_8971','Toyota Lexus','Classic Cars','1:10','Unimax Art Galleries','speedy','500','50','54.60');

Order
INSERT INTO classicmodels.orders (orderNumber, orderDate, requiredDate, shippedDate, status, comments, customerNumber) VALUES (10500, '2013-01-09', '2013-01-18', null, 'On Hold', 'Check on availability.', 128);
INSERT INTO classicmodels.orderdetails (orderNumber, productCode, quantityOrdered, priceEach, orderLineNumber) VALUES (10500, 'S18_1749', 30, 136.00, 3);
INSERT INTO classicmodels.orderdetails (orderNumber, productCode, quantityOrdered, priceEach, orderLineNumber) VALUES (10500, 'S18_2248', 50, 55.09, 2);


SELECT * FROM mysql.general_log;
                                                                    |
select * from user limit 10;

| %         | Bookkeeping      | N           | N           | N           | N           | N           | N         | N           | N             | N            | N         | N          | N               | N          | N          | N            | N          | N                     | N                | N            | N               | N                | N                | N              | N                   | N                  | N                | N          | N            | N                      |          |            |             |              |             0 |           0 |               0 |                    0 | caching_sha2_password | $A$005$8{"6-A)Eoi r8/JupWXOkta2apDwkUF/prERPOjx/kUKjrvfvEBgoFC3 | N                | 2019-02-24 22:36:09   |              NULL | N              | N                | N              |                   NULL |                NULL | NULL                     | NULL            |
| %         | Inventory        | N           | N           | N           | N           | N           | N         | N           | N             | N            | N         | N          | N               | N          | N          | N            | N          | N                     | N                | N            | N               | N                | N                | N              | N                   | N                  | N                | N          | N            | N                      |          |            |             |              |             0 |           0 |               0 |                    0 | caching_sha2_password | $A$005$+enut.4knn_rOV=@|2inGG6Z0W1IwPC727LSR92kSrHxmLkmuLhxMsXvPpBe0 | N                | 2019-02-24 22:35:50   |              NULL | N              | N                | N              |                   NULL |                NULL | NULL                     | NULL            |
| %         | Sales            | N           | N           | N           | N           | N           | N         | N           | N             | N            | N         | N          | N               | N          | N          | N            | N          | N                     | N                | N            | N               | N                | N                | N              | N                   | N                  | N                | N          | N            | N                      |          |            |             |              |             0 |           0 |               0 |                    0 | caching_sha2_password | $A$005$fPhdV%Oq]YC.VL8w5tFWt.2C2ac9VRvSwksNjTGg1YAS/hkmuBn95OZb3 | N                | 2019-02-24 22:36:32   |              NULL | N              | N                | N              |                   NULL |                NULL | NULL                     | NULL            |
| %         | humanresource    | N           | N           | N           | N           | N           | N         | N           | N             | N            | N         | N          | N               | N          | N          | N            | N          | N                     | N                | N            | N               | N                | N                | N              | N                   | N                  | N                | N          | N            | N                      |          |            |             |              |             0 |           0 |               0 |                    0 | caching_sha2_password | $A$005${FcB_I7Ez#7|rXM/XdECiatFMCOfl10FXfBq35kfIWfUKg98RSGQBvGYV8 | N                | 2019-02-24 22:36:21   |              NULL | N              | N                | N              |                   NULL |                NULL | NULL                     | NULL            |
| %         | root             | Y           | Y           | Y           | Y           | Y           | Y         | Y           | Y             | Y            | Y         | Y          | Y               | Y          | Y          | Y            | Y          | Y                     | Y                | Y            | Y               | Y                | Y                | Y              | Y                   | Y                  | Y                | Y          | Y            | Y                      |          |            |             |              |             0 |           0 |               0 |                    0 | caching_sha2_password | $A$005$b?l&7p:sVN0[62!hv(VxtORfJ.zbmHxbP1jI8VzT9D6Rdy/kVWE8kHZbO6zs3 | N                | 2019-02-24 15:43:15   |              NULL | N              | Y                | Y              |                   NULL |                NULL | NULL                     | NULL            |
| localhost | IT               | N           | N           | N           | N           | N           | N         | N           | N             | N            | N         | N          | N               | N          | N          | N            | N          | N                     | N                | N            | N               | N                | N                | N              | N                   | N                  | N                | N          | N            | N                      |          |            |             |              |             0 |           0 |               0 |                    0 | caching_sha2_password | $A$005$vkD,t[S%C!*L2BUM1YnYA7rM9SfsSUzVTyzeY47upYEaasOj3jokcBW3 | N                | 2019-02-24 22:36:45   |              NULL | N              | N                | N              |                   NULL |                NULL | NULL                     | NULL            |
| localhost | mysql.infoschema | Y           | N           | N           | N           | N           | N         | N           | N             | N            | N         | N          | N               | N          | N          | N            | N          | N                     | N                | N            | N               | N                | N                | N              | N                   | N                  | N                | N          | N            | N                      |          |            |             |              |             0 |           0 |               0 |                    0 | caching_sha2_password | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED | N                | 2019-02-24 15:43:05   |              NULL | Y              | N                | N              |                   NULL |                NULL | NULL                     | NULL            |
| localhost | mysql.session    | N           | N           | N           | N           | N           | N         | N           | N             | N            | N         | N          | N               | N          | N          | N            | Y          | N                     | N                | N            | N               | N                | N                | N              | N                   | N                  | N                | N          | N            | N                      |          |            |             |              |             0 |           0 |               0 |                    0 | caching_sha2_password | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED | N                | 2019-02-24 15:43:05   |              NULL | Y              | N                | N              |                   NULL |                NULL | NULL                     | NULL            |
| localhost | mysql.sys        | N           | N           | N           | N           | N           | N         | N           | N             | N            | N         | N          | N               | N          | N          | N            | N          | N                     | N                | N            | N               | N                | N                | N              | N                   | N                  | N                | N          | N            | N                      |          |            |             |              |             0 |           0 |               0 |                    0 | caching_sha2_password | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED | N                | 2019-02-24 15:43:05   |              NULL | Y              | N                | N              |                   NULL |                NULL | NULL                     | NULL            |
| localhost | root             | Y           | Y           | Y           | Y           | Y           | Y         | Y           | Y             | Y            | Y         | Y          | Y               | Y          | Y          | Y            | Y          | Y                     | Y                | Y            | Y               | Y                | Y                | Y              | Y                   | Y                  | Y                | Y          | Y            | Y                      |          |            |             |              |             0 |           0 |               0 |                    0 | caching_sha2_password | $A$005$&}[4t
                                             0-6R5'3BjY47y7UvdAgB1Ra/c8ykWUfwqn/tyMmjYAXSv5jTPW/ | N                | 2019-02-24 15:43:15   |              NULL | N              | Y                | Y              |                   NULL |                NULL | NULL                     | NULL            |


FROM mysql.general_log
ORDER BY event_time DESC
LIMIT 10 |
| 2019-02-24 23:26:20.999344 | root[root] @ localhost [] | Query        | select * from user limit 10                                                                                    |
| 2019-02-24 23:24:20.364930 | root[root] @ localhost [] | Query        | select * from columns_priv limit 10                                                                            |
| 2019-02-24 23:23:52.382875 | root[root] @ localhost [] | Query        | select * from default_roles limit 10                                                                           |
| 2019-02-24 23:23:28.315017 | root[root] @ localhost [] | Query        | select * from role_edges limit 10                                                                              |
| 2019-02-24 23:22:41.942343 | root[root] @ localhost [] | Query        | select * from global_grants limit 10                                                                           |
| 2019-02-24 23:21:55.062405 | root[root] @ localhost [] | Query        | show tables                                                                                                    |
| 2019-02-24 23:17:52.158454 | root[root] @ localhost [] | Query        | select * from general_log                                                                                      |
| 2019-02-24 23:17:22.832534 | root[root] @ localhost [] | Query        | show tables                                                                                                    |
| 2019-02-24 23:17:16.166173 | root[root] @ localhost [] | Field List   | user             


The above shows when the user was created.

One attempt by a bookkeeping user with wrong privileges:
mysql -u Bookkeeping -ppassword

INSERT INTO classicmodels.employees (employeeNumber,lastname, firstname, extension, email, officeCode, reportsTo, jobTitle) values (1704, 'fuskersen','svindlergert','x1000','gj@bedrag.com','1','1002','fingeren');

ERROR 1142 (42000): INSERT command denied to user 'Bookkeeping'@'localhost' for table 'employees'

The attempted insert will be denied as this user is not privileges. 

Excercise 3

The syntax for full back up of all the databases is as follow:

mysqldump --all-databases > dump.sql.

To restore all of the databases is as follows:
mysql < dump.sql.

The back up should be stored outside the docker container so one do not riscing to loose the data.




