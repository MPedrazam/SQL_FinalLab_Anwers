PART 1.

1. List  the names of the cities in alphabetical order where Classic Models has offices.

SELECT DISTINCT city
    FROM "alanparadise/cm"."offices"
    ORDER BY city;

2. List the EmployeeNumber, LastName, FirstName, Extension for all employees working out of the Paris office.

SELECT employeenumber, lastname, firstname, extension
    FROM "alanparadise/cm"."employees" A JOIN "alanparadise/cm"."offices" B
        ON A.officecode = B.officecode
    WHERE city LIKE 'Paris';

3. List the ProductCode, ProductName, ProductVendor, QuantityInStock and ProductLine for all products with a QuantityInStock between 200 and 1200. 

SELECT ProductCode, ProductName, ProductVendor, QuantityInStock, ProductLine
    FROM "alanparadise/cm"."products"
    WHERE quantityinstock < 1200 and quantityinstock > 200;

4. (Use a SUBQUERY) List the ProductCode, ProductName, ProductVendor, BuyPrice and MSRP for the least expensive (lowest MSRP) product sold by ClassicModels.  (“MSRP” is the Manufacturer’s Suggested Retail Price.)  

SELECT ProductCode, ProductName, ProductVendor, BuyPrice, MSRP 
    FROM "alanparadise/cm"."products"
    WHERE  MSRP IN (SELECT MIN(MSRP)
            FROM "alanparadise/cm"."products");

5. What is the ProductName and Profit of the product that has the highest profit (profit = MSRP minus BuyPrice)

SELECT ProductName, (MSRP - BuyPrice) AS Profict 
    FROM "alanparadise/cm"."products"
    WHERE (MSRP - BuyPrice) IN (SELECT MAX((MSRP - BuyPrice))
        FROM "alanparadise/cm"."products");

6. List the country and the number of customers from that country for all countries having just two  customers.  List the countries sorted in ascending alphabetical order. Title the column heading for the count of customers as “Customers”. 

SELECT Country, COUNT(Country) AS Customers
    FROM "alanparadise/cm"."Customers"
    GROUP BY country
          HAVING COUNT(Country) = 2
    ORDER BY country ASC;

7. List the ProductCode, ProductName, and number of orders for the products with exactly 25 orders.  Title the column heading for the count of orders as “OrderCount”.  

SELECT ProductCode, ProductName, COUNT(productcode) AS OrderCount
    FROM "alanparadise/cm"."products" JOIN "alanparadise/cm"."orderdetails" ON
    "alanparadise/cm"."products".productcode = "alanparadise/cm"."orderdetails".productcode
    GROUP BY OrderCount; ProductName
      HAVING OrderCount = 25; 

8. List the EmployeeNumber, Firstname + Lastname  (concatenated into one column in the answer set, separated by a blank and referred to as ‘name’) for all the employees reporting to Diane Murphy or Gerard Bondur. 

SELECT EmployeeNumber, CONCAT(Firstname, ' ', Lastname) AS Name
    FROM "alanparadise/cm"."employees"
    WHERE reportsto IN 
        (SELECT  EmployeeNumber
            FROM "alanparadise/cm"."employees" 
            WHERE (lastname LIKE 'Murphy' and firstname LIKE 'Diane') OR (lastname LIKE 'Bondur' and firstname LIKE 'Gerard')
        );

9. List the EmployeeNumber, LastName, FirstName of the president of the company (the one employee with no boss.) 

SELECT EmployeeNumber, LastName, FirstName
     FROM "alanparadise/cm"."employees"
     WHERE reportsto ISNULL

10. List the ProductName for all products in the “Classic Cars” product line from the 1950’s.

SELECT ProductName
     FROM "alanparadise/cm"."products"
     WHERE productline LIKE 'Classic Cars' and ProductName LIKE '%195%';

11. List the month name and the total number of orders for the month in 2004 in which ClassicModels customers placed the most orders.

SELECT MonthName(OrderDate) as OrderMonth  , COUNT(OrderNumber)
	FROM  "alanparadise/cm"."orders" 
	WHERE EXTRACT(year from OrderDate) = '2004'
GROUP BY OrderMonth 
ORDER BY 1 DESC Limit 1;

12. List the firstname, lastname of employees who are Sales Reps who have no assigned customers.  

SELECT LastName, FirstName 
	FROM "alanparadise/cm"."employees" Left Outer JOIN "alanparadise/cm"."customers" 
		ON "alanparadise/cm"."employees".employeenumber = "alanparadise/cm"."customers" .salesrepemployeenumber
	WHERE CustomerName ISNULL  
		and jobtitle = "Sales Rep";

13. List the customername of customers from Switzerland with no orders. 

SELECT CustomerName, Country
	FROM "alanparadise/cm"."customers"  Left Outer JOIN "alanparadise/cm"."orders"
		ON "alanparadise/cm"."customers".customernumber = "alanparadise/cm"."orders".customernumber
	WHERE "alanparadise/cm"."orders".customernumber ISNULL and Country = 'Switzerland';

14. List the customername and total quantity of products ordered for customers who have ordered more than 1650 products across all their orders.

SELECT CustomerName, SUM(QuantityOrdered) as Totalq
	FROM "alanparadise/cm"."customers" JOIN  "alanparadise/cm"."orders"
		ON "alanparadise/cm"."customers".CustomerNumber = "alanparadise/cm"."orders".CustomerNumber
         	JOIN "alanparadise/cm"."orderdetails" 
			ON "alanparadise/cm"."customers".OrderNumber = "alanparadise/cm"."orderdetails".ordernumber 
	GROUP BY Customername
		HAVING Totalq > 1650;
		
PART 2

1. Create a NEW table named “TopCustomers” with three columns: CustomerNumber (integer), ContactDate (DATE) and  OrderTotal (a real number.)  None of these columns can be NULL.

CREATE TABLE TopCustomers ( 
 		CustomerNumber  int not null,  
		ContactDate	DATE not null,
		OrderTotal	decimal(9,2) not null default 0, 
		constraint	PKTopCustomers primary key (CustomerNumber) 
 		);

2. Populate the new table “TopCustomers” with the CustomerNumber, today’s date, and the total value of all their orders (PriceEach * quantityOrdered) for those customers whose order total value is greater than $140,000.

INSERT INTO TopCustomers 
	SELECT TopCustomers.CustomerNumber, CURRENT_date,
		SUM(PriceEach * Quantityordered) 
 	   	FROM Customers c, Orders o, OrderDetails d  	 	
		WEHERE c.CustomerNumber = o.CustomerNumber AND o.Ordernumber = d.Ordernumber 
 	GROUP BT c.Customernumber
		HAVING SUM(PriceEach * Quantityordered) > 140000;

3. List the contents of the TopCustomers table in descending OrderTotal sequence.
	
SELECT *
	FROM TopCustomers
	ODER BY OrderTotal DESC; 

4. Add a new column to the TopCustomers table called OrderCount.

ALTER TABLE TopCustomers
	ADD Column OrderCount integer; 


5. Update the Top Customers table, setting the OrderCount to a random number between 1 and 10.

UPDATE TopCustomers
	SET OrderCount = floor((rand()*18));

6. List the contents of the TopCustomers table in descending OrderCount sequence.

SELECT * 
	FROM TopCustomers 
	ORDER BY OrderCount DESC;

7. Drop the TopCustomers table.

DROP TABLE TopCustomers; 
