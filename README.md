
Question 1: Given some sample data, write a program to answer the following: click here to access the required data set

On Shopify, we have exactly 100 sneaker shops, and each of these shops sells only one model of shoe. We want to do some analysis of the average order value (AOV). When we look at orders data over a 30 day window, we naively calculate an AOV of $3145.13. Given that we know these shops are selling sneakers, a relatively affordable item, something seems wrong with our analysis. 

Think about what could be going wrong with our calculation. Think about a better way to evaluate this data. 
What metric would you report for this dataset?
What is its value?



ANS: AOV the average order value is caluclated as the total revenue divided by the #orders. However, after inspetion of the data, I noticed that there are two types of customers. Businesses usally requesting in thousands and normal clients requesting in under a dozen.

After splitting the data into B2B and B2C, I have calculated the AOV for both and got the following results:

#('AOV of B2B is ', 704000.0) #('AOV of B2C is ', 754.0919125025085)

Seems more reasonable for B2C now.

Python Code:
import subprocess

# the main idea is to notice that there are two diffrent types of clients. the first type buy items under a dozen while the sceond type by items in thousands therefore the average does not reflect real orders because the sample is not coherent
#  the solution would be to divide the sample and calculate two diffrent averages: B2C and B2B

#define main
def main():

	#open the newly created input file
	B2Bprice =0.0
	B2Cprice = 0.0
	B2Borders =0.0
	B2Corders = 0.0
	with open("sheet.tsv", "r") as file:
		for line in file:
			temp1, temp2, temp3, value, quantity, temp4, temp5, temp6 = line.split()
			value = int(value)
			quantity = int(quantity)

			if (quantity > 1000): #this is to differentiate between the two cases
				B2Bprice += value
				B2Borders += 1
			else:
				B2Cprice += value
				B2Corders += 1

	print("AOV of B2B is " , B2Bprice / B2Borders)
	print("AOV of B2C is " , B2Cprice / B2Corders)
#define run
if __name__ == "__main__":
	main()

Hello :) :)

AOV the  average order value is caluclated as the total revenue divided by the #orders. However, after inspetion of the data, I noticed that there 
are two types of customers. Businesses usally requesting in thousands and normal clients requesting in under a dozen.

After splitting the data into B2B and B2C, I have calculated the AOV for both and got the following results:


('AOV of B2B is ', 704000.0)
('AOV of B2C is ', 754.0919125025085)

Seems more reasonable for B2C now.

Thank you for your time and consideration

Q-2) For this question you’ll need to use SQL. Follow this link to access the data set required for the challenge. Please use queries to answer the following questions. Paste your queries along with your final numerical answers below.

How many orders were shipped by Speedy Express in total?
What is the last name of the employee with the most orders?
What product was ordered the most by customers in Germany?


Ans-2 
SHOW DATABASES;
SHOW TABLES;

-- a) How many orders were shipped by Speedy Express in total?

CREATE VIEW Shipper_Orders AS
SELECT Orders.OrderID, Orders.ShipperID, Shippers.ShipperName
FROM Orders 
JOIN Shippers
ON Shippers.ShipperID=Orders.ShipperID;

SELECT COUNT(*) FROM [Shipper_Orders]
WHERE ShipperName = 'Speedy Express';

ANSWER: 54

-- b) What is the last name of the employee with the most orders? 

CREATE VIEW Employee_Orders AS 
SELECT Orders.EmployeeID, Employees.LastName, Orders.OrderID
FROM Orders
JOIN Employees
ON Employees.EmployeeID=Orders.EmployeeID; 

SELECT LastName, COUNT(*)
FROM Employee_Orders
GROUP BY LastName
ORDER BY COUNT(*) desc;

ANSWER: Peacock, 40 

-- c) What product was ordered the most by customers in Germany?

CREATE VIEW Products_Ordered AS
SELECT Orders.OrderID, Customers.Country, OrderDetails.Quantity, Products.ProductName
FROM Orders, OrderDetails
JOIN Customers ON Orders.CustomerID=Customers.CustomerID
JOIN Products ON OrderDetails.ProductID=Products.ProductID
WHERE Country='Germany';

CREATE VIEW Product_Orders AS
SELECT ProductName, Quantity, COUNT(*) as 'Orders'
FROM Products_Ordered
GROUP BY ProductName;

SELECT ProductName, Quantity, Orders, (Quantity * Orders) AS TotalOrdered
FROM Product_Orders
ORDER BY TotalOrdered desc
LIMIT 1;

Answer:  

ProductName - Camembert Pierrot
Quantity - 40 
Orders - 300
TotalOrdered - 12000
