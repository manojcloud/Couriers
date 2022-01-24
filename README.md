# Couriers
TODO: Add some badges in here

TODO: Add Table of contents

## Overview

The goal of the 'Couriers' project is to help a delivery company manage orders by utilizing an SQL database.

The system has the ability to ```create```, ```read```, ```update```, and ```delete``` (CRUD) the data from tables which store information about orders, clients, dispatchers, orders, etc.
(more info can be found in the Database Design part of this documentation)

## Database Design

We designed the schema of our database ```CouriersDB``` by transforming a table in 1FN[^1] into 7 tables that meet the 3NF[^2] standards.

This is the given 1FN table with some sample data:

| Order # | Order Date | Dispatcher Name | Phone Number - Dispatcher | Client Name | Phone Number - Client | Type of Service | Total | Courier Name | Phone Number - Courier | Delivery Address | Recipient Name | Delivery Date |
| ---------------- | --------------- | ---------------- | ------------------ | -------------- | ---------------- | ---------------------------- | -------- | ------------- | ---------------- | ------------------------- | ---------------- | ---------------- |
| 1                | 8/16/2021       | Асен Донев       | 0887897555         | Галин Христов  | 0888555111       | Колетна пратка над 2 до 5 кг | 6.72 лв. | Димана Донева | 0876555555       | ул. Бреза 9               | Галена Халиева   | 20.8.2021 г.     |
| 2                | 9/9/2021        | Асен Донев       | 0887897555         | Джим Шон       | 0888555222       | Колетна пратка до 2 кг       | 6.00 лв. | Камен Каменов | 0876555111       | ул. Александър Кръстев 22 | Шон Джим         | 15.9.2021 г.     |
| 3                | 9/9/2021        | Колю Колев       | 0888987555         | Джим Шон       | 0888555222       | Колетна пратка над 2 до 5 кг | 6.72 лв. | Мони Иванова  | 0876555222       | ул. Вардар 8              | Шон Джим         | 15.9.2021 г.     |
| 4                | 9/14/2021       | Маша Малишкина   | 0881122678         | Галин Христов  | 0888555111       | Колетна пратка над 2 до 5 кг | 6.72 лв. | Димана Донева | 0876555555       | ул. Бук 18                | Христо Галев     | 17.9.2021 г.     |

The ```CouriersDB```, which consists of 7 3NF tables has the following schema:

-- IMAGE_SCHEMA


### **dbo.Orders**

```sql
CREATE TABLE Orders (
	ID int identity(1, 1) NOT NULL primary key,
	OrderDate date NOT NULL,
	ReceiveDate date NOT NULL,
	Total money NOT NULL,
	AddressID int FOREIGN KEY references Addresses(ID),
	TypeID int FOREIGN KEY references TypesOfService(ID),
	DispatcherID int FOREIGN KEY references Dispatchers(ID),
	ClientID int FOREIGN KEY references Clients(ID),
	CourierID int FOREIGN KEY references Couriers(ID),
	RecipientID int FOREIGN KEY references Recipients(ID),
)
```

| Column Name | Data Type | Descriprtion |
| ---------------- | --------------- | ----------- |
| ID | INT | The identification number (```PRIMARY KEY```) |
| OrderDate | DATE | The date of the registration of the order |
| ReceiveDate | DATE | The date of the delivery of the order |
| AddressID | INT | The identification number of the Address, where the order has to be delivered to (```FOREIGN KEY``` linked to the ```ID``` of (```dbo.Addresses```) |
| TypeID | INT | The identification number of the type of service of the order (```FOREIGN KEY``` linked to the ```ID``` of ```dbo.TypesOfService```) |
| DispatcherID | INT | The identification number of the dispatcher, who works on the coordination of the order (```FOREIGN KEY``` linked to the ```ID``` of ```dbo.Dispatchers```) |
| ClientID | INT | The identification number of the client, who has made the order (``````FOREIGN KEY`````` linked to the ```ID``` of ```dbo.Clients```) |
| CourierID | INT | The identification number of the type of service of the courier, who has to deliver the order (```FOREIGN KEY``` linked to the ```ID``` of ```dbo.Couriers```) |
| RecipientID | INT | The identification number of the recipient, who has to receive the order (```FOREIGN KEY``` linked to the ```ID``` of ```dbo.Recipients```) |

### **dbo.Clients**

```sql
CREATE TABLE Clients (
	ID int identity(1, 1) NOT NULL primary key,
	Name varchar(50) NOT NULL,
	PhoneNumber varchar(10) NOT NULL,
	unique(Name, PhoneNumber)
)
```

>NOTE: The ```Name``` and ```PhoneNumber``` columns are combined into a composite ```unique``` key to ensure that there are not any duplicate records when inserting values into ```dbo.Clients```

| Column Name | Data Type | Descriprtion |
| ---------------- | --------------- | ----------- |
| ID | INT | The identification number (```PRIMARY KEY```) |
| Name | VARCHAR(50) | The name of the client |
| PhoneNumber | VARCHAR(10) | The phone number of the client |

### **dbo.Dispatchers**

```sql
CREATE TABLE Dispatchers (
	ID int identity(1, 1) NOT NULL primary key,
	Name varchar(50) NOT NULL,
	PhoneNumber varchar(10) NOT NULL,
	unique(Name, PhoneNumber)
)
```

>NOTE: The ```Name``` and ```PhoneNumber``` columns are combined into a composite ```unique``` key to ensure that there are not any duplicate records when inserting values into ```dbo.Dispatchers```

| Column Name | Data Type | Descriprtion |
| ---------------- | --------------- | ----------- |
| ID | INT | The identification number (```PRIMARY KEY```) |
| Name | VARCHAR(50) | The name of the dispatcher |
| PhoneNumber | VARCHAR(10) | The phone number of the dispatcher |

### **dbo.Couriers**

```sql
CREATE TABLE Couriers (
	ID int identity(1, 1) NOT NULL primary key,
	Name varchar(50) NOT NULL,
	PhoneNumber varchar(10) NOT NULL,
	unique(Name, PhoneNumber)
)
```

>NOTE: The ```Name``` and ```PhoneNumber``` columns are combined into a composite ```unique``` key to ensure that there are not any duplicate records when inserting values into ```dbo.Couriers```

| Column Name | Data Type | Descriprtion |
| ---------------- | --------------- | ----------- |
| ID | INT | The identification number (```PRIMARY KEY```) |
| Name | VARCHAR(50) | The name of the courier |
| PhoneNumber | VARCHAR(10) | The phone number of the courier |

### **dbo.Recipients**

```sql
CREATE TABLE Recipients (
	ID int identity(1, 1) NOT NULL primary key,
	Name varchar(50) NOT NULL
)
```

| Column Name | Data Type | Descriprtion |
| ---------------- | --------------- | ----------- |
| ID | INT | The identification number (```PRIMARY KEY```) |
| Name | VARCHAR(50) | The name of the recipient |

### **dbo.Addresses**

```sql
CREATE TABLE Addresses (
	ID int identity(1, 1) NOT NULL primary key,
	Address varchar(50) NOT NULL unique,
)
```

| Column Name | Data Type | Descriprtion |
| ---------------- | --------------- | ----------- |
| ID | INT | The identification number (```PRIMARY KEY```) |
| Address | VARCHAR(50) | The details (street, number, etc.) of the address |

### **dbo.TypesOfService**

```sql
CREATE TABLE TypesOfService (
	ID int identity(1, 1) NOT NULL primary key,
	Type varchar(50) NOT NULL unique,
)
```

| Column Name | Data Type | Descriprtion |
| ---------------- | --------------- | ----------- |
| ID | INT | The identification number (```PRIMARY KEY```) |
| Type | VARCHAR(50) | The type of the service that needs to be performed |

## Project Structure

```
📦Couriers
 ┣ 📂source
 ┃ ┣ 📂crud-procedures
 ┃ ┃ ┣ 📄add-client.sql
 ┃ ┃ ┣ 📄delete-client.sql
 ┃ ┃ ┣ 📄update-client.sql
 ┃ ┃ ┗ ...
 ┃ ┣ 📂data
 ┃ ┃ ┣ 📄addresses-data.sql
 ┃ ┃ ┣ 📄clients-data.sql
 ┃ ┃ ┗ ...
 ┃ ┣ 📂private
 ┃ ┃ ┗ 📄check-phone-number.sql
 ┃ ┣ 📂queries
 ┃ ┃ ┣ 📄OutRequest1.sql
 ┃ ┃ ┣ 📄OutRequest2.sql
 ┃ ┃ ┣ 📄OutRequest3.sql
 ┃ ┃ ┣ 📄OutRequest4.sql
 ┃ ┃ ┣ 📄OutRequest5.sql
 ┃ ┃ ┣ 📄usp_dates_with_most_delivered_orders.sql
 ┃ ┃ ┣ 📄usp_names_of_recipients_by_order_count.sql
 ┃ ┃ ┣ 📄usp_name_phonenumber_category.sql
 ┃ ┃ ┣ 📄usp_orders_count_by_order_date.sql
 ┃ ┃ ┗ 📄usp_orders_profit_by_tos.sql
 ┃ ┗ 📂schemas
 ┃ ┃ ┣ 📄addresses-table.sql
 ┃ ┃ ┣ 📄clients-table.sql
 ┃ ┃ ┗ ...
 ┣ 📂sql-compiler
 ┃ ┣ 📄Program.cs
 ┃ ┣ 📄sql-compiler.csproj
 ┃ ┗ 📄sql-compiler.exe
 ┣ 📄.gitignore
 ┣ 📄compile.bat
 ┣ 📄couriers-project_data.xlsx
 ┣ 📄couriers.sql
 ┣ 📄examples.sql
 ┗ 📄README.md
```

In order to facilitate for easier collaboration and overall code development, we split up all of the SQL code into many files. All of them are located in the ```source``` folder in the root of the repository. In there you will find the files grouped into the ```crud-procedures```, ```data```, ```private```, ```queries``` and ```schemas``` folders. All of those folders contain a number of ```.sql``` files, which themselves are all either a definition of a single procedure, function, table or database, or in the case of the files in the ```data``` folder - the ```INSERT``` queries used to populate the different tables with their respective data.

NON BREAKING HYPHEN ‑ (COPY PASTE FROM HERE)
table generator: https://www.tablesgenerator.com/markdown_tables

Here's a brief description of what each of the folders contain:
| Folder | Contains |
|:---:|---|
| ```crud‑procedures``` | CRUD[^crud] procedures, except for the Read part, so basically different procedures for every table, which can each ```Add```, ```Update``` or ```Delete``` any data within that table |
| ```data``` | SQL queries which insert the original data from the Excel table (```couriers‑project_data.xlsx```) into the different tables of the database |
| ```private``` | Procedures and functions which are not meant to be used by the end user, but rather by other procedures and functions in the database |
| ```queries``` | Various procedures which read certain data from the database and display it to the user in different ways |
| ```schemas``` | SQL queries which create each of the tables in the database and the database itself |

The folder ```sql-compiler``` contains the source code and executable for a small SQL "compiler", written in C#. All it does is it takes all of the SQL files (in a certain order) and it combines them into a single SQL file. This makes it easy for both the developers and the end-user to execute all of the files at once and immediately have the database up and running, while at the same time allowing for the separation of the SQL code, which as we mentioned earlier facilities for more efficient development and code cleanliness. Another thing that the compiler does is that it creates a single file with all of the examples for each SQL file, which contains such examples. This makes it much easier for the end user to get to grips with the various things that they can do with the database, while also making it easier for developers to write examples as it allows them to do so within the actual file that they are working on at the moment.

To prevent clutter, the actual C# project for the compiler isn't a part of a Visual Studio Solution File (```.sln```) as it was created with the ```dotnet``` CLI tool which only requires a C# Project File (```sql-compiler.csproj```) and a C# Source File (```Program.cs```). The exe is a completely standalone executable that doesn't require the .NET runtime to work as it was created with the very useful .NET tool called [```dotnet-warp```](https://www.nuget.org/packages/dotnet-warp/).

To actually use the compiler you can pass arguments to it, but you can simply run the ```compile.bat``` file which will run the compiler with some default arguments. You can configure those arguments within the file and also you can configure which SQL files are compiled and in what order that is done, as that may be important depending on the files (e.g. you'd want the database to be created, before you create the tables). That's not actually passed to the compiler as an argument, but rather the ```compile.bat``` file passes itself as an argument and the compiler reads a comment within the ```bat``` file which contains the names and relative paths of the files which are to be compiled (you can also use glob style wildcards such as `*` and `**`, just like in a `.gitignore` file).

By default, the compiler generates 2 SQL files with the names `couriers.sql` and `example.sql` in the root directory of the repository. However, they are included in the `.gitignore` of the repo as to not cause any merge conflicts.



## Guide

## Development

### **Create Procedures**

### **Read Procedures**

```CouriersDB``` has 10 read procedures. Each one of them is used as a report, containing information about:

1. a client's ```Name``` and ```PhoneNumber``` via searching by part of ```Name``` or ```PhoneNumber``` (columns in ```dbo.Clients```)

TODO:

2. all orders of a specific dispatcher by ```Name``` (column in ```dbo.Dispatchers```) or on a certain ```ReceiveDate``` (column in ```dbo.Order```)

TODO:

3. the ```Total``` (column in ```dbo.Order```) of all orders by a specific customer

TODO:

4. all orders made on a specific ```OrderDate``` (column in ```dbo.Order```)

TODO:

5. all orders of a specific courier by ```Name``` (column in ```dbo.Courier```)

TODO:

6. the date(s) with most orders grouped by ```ReceiveDate``` (column in ```dbo.Orders```)

```sql
CREATE OR ALTER Proc usp_dates_with_most_delivered_orders
AS
BEGIN
	SELECT COUNT(*) AS [count]
	INTO TempOrders
	FROM Orders
	GROUP BY ReceiveDate;

	SELECT ReceiveDate, COUNT(*) as [Count of delivered orders]
	FROM Orders AS o1
	GROUP BY ReceiveDate
	HAVING COUNT(*) >= ALL (
		SELECT [count]
		FROM TempOrders);
END
```

### Work Principle

The first ```SELECT``` statement inside ```usp_dates_with_most_delivered_orders``` creates a temporary table (```dbo.TempOrders```) and inserts into that table the ```COUNT``` of all orders grouped by ```ReceiveDate```.

The second ```SELECT``` statement displays the ```COUNT``` of the dates(s), during which most orders were received, as well as the ```ReceiveDate``` themselves. This happens by grouping the records by ```ReceiveDate```. Then, only those records, which have ```COUNT(*)``` greater or equal to all the other records, are shown. Using ```>= ALL``` guarantees that only those date(s), which has/have the largest ```COUNT(*)```, are being shown.

### Usage

```sql
EXEC dbo.usp_dates_with_most_delivered_orders;
```

7. the names of all the recipients who have received more orders than ```@MinOrdersCount```

```sql
CREATE OR ALTER Proc usp_names_of_recipients_by_order_count @MinOrdersCount INT
AS
BEGIN
	SELECT COUNT(RecipientID) AS [Count of orders], Name
	FROM Orders
	JOIN Recipients
	ON RecipientID = Recipients.ID
	GROUP BY Name
	HAVING COUNT(RecipientID) > @MinOrdersCount
END
```

### Work Principle

The ```SELECT``` statement displays the ```COUNT``` of the orders and ```Name``` (column in ```dbo.Recipients```). Those orders are grouped by ```Name``` (column in ```dbo.Recipients```). Then, only the records with ```COUNT``` greater that ```@MinOrdersCount``` are shown.

>NOTE: ```@MinOrdersCount``` (```INT```) + 1 is the minimum amount of orders that a recipient must receive in order to be displayed when ```usp_names_of_recipients_by_order_count``` is executed

### Usage

```sql
EXEC dbo.usp_names_of_recipients_by_order_count 1;
```
```sql
EXEC dbo.usp_names_of_recipients_by_order_count 2;
```

8. the count of all the orders grouped by ```ReceiveDate``` (column in ```dbo.Order```)

```sql
CREATE OR ALTER Proc usp_orders_count_by_order_date
AS
BEGIN
	SELECT COUNT(*) AS [Count of Orders], OrderDate
	FROM Orders
	GROUP BY OrderDate
END
```

### Work Principle

The ```SELECT``` statement displays the ```COUNT``` of the orders, grouped by ```OrderDate```, as well as the ```OrderDate```.

### Usage

```sql
EXEC dbo.usp_orders_count_by_order_date;
```

9. the profit of all the orders grouped by ```Type[^3]``` (column in ```dbo.TypesOfService```)

```sql
CREATE OR ALTER Proc usp_orders_profit_by_tos
AS
BEGIN
	SELECT FORMAT(SUM(Total), 'C', 'bg-BG') AS [Profit], Type
	FROM Orders
	JOIN TypesOfService
	ON Orders.TypeID = TypesOfService.ID
	GROUP BY Type
END
```

### Work Principle

The ```SELECT``` statement displays the ```SUM``` of the totals of the orders, grouped by ```Type``` (column in ```dbo.TypesOfService```), as well as the ```Type```.

>NOTE: The function ```FORMAT``` is used to display the ```SUM``` in BGN[^4].

### Usage

```sql
EXEC dbo.usp_orders_profit_by_tos;
```

10. the names, phone numbers, and categories (client, dispatcher, courier) of all the people registered in ```CouriersDB```

```sql
CREATE OR ALTER Proc usp_name_phonenumber_category
AS
BEGIN
	SELECT Name, PhoneNumber, 'Courier' AS [Category]
	FROM Couriers
	UNION
	SELECT Name, PhoneNumber, 'Client'
	FROM Clients
	UNION
	SELECT Name, PhoneNumber, 'Dispatcher'
	FROM Dispatchers
END
```

### Work Principle

The ```SELECT``` statement displays ```Name```, ```PhoneNumber```, and ```Category```. The data is fetched from the union of three tables (```dbo.Courier```, ```dbo.Client```, and ```dbo.Dispatcher```).

### Usage

```sql
EXEC dbo.usp_name_phonenumber_category;
```

### **Update Procedures**

### **Delete Procedures**

```CouriersDB``` has 2 types of delete procedures:

### *Type 1:* Delete procedures for parent tables

```CouriersDB``` has 6 parent tables (```dbo.Clients```, ```dbo.Dispatchers```, ```dbo.Couriers```, ```dbo.Recipients```, ```dbo.Addresses```, and ```dbo.TypesOfService```).

>NOTE: The following sql statements are part of the **delete procedure** ```dbo.delete_couriers```. Given that all delete procedures for the parent tables almost completely overlap, it is unnecessary to show all 6 of them here.

For each one of those parent tables we have created a **delete procedure**, which deletes the records from a specific parent table by a given ```@OldID``` (unless ```@OldID``` is invalid, which is a case discussed below).

```sql
DELETE FROM Couriers
WHERE ID = @OldID;
```

In addition to the above and based on the entered parameters, the type-1 delete procedures can be executed in 5 different ways:

1. If the user enters an invalid ```@OldID```

In this case, the procedure will ```PRINT``` a message which says: 'No such [some object] exists', and the procedure will exit without making any alterations to any of the tables of ```CouriersDB```

```sql
IF NOT EXISTS (SELECT * FROM Couriers WHERE ID = @OldID)
BEGIN
	PRINT 'No such courier exists';
	RETURN
END;
```

2. If the user enters only a valid ```@OldID```
```sql
EXEC dbo.delete_couriers 1;
```

In this case, the procedure will ```DELETE``` the records from ```dbo.Orders``` where the ```FOREIGN KEY``` (```courierID``` in the given example) linked to a specific table is equal to ```@OldID```
```sql
IF (@WantToDeleteFromOrders = 1)
BEGIN
	SET @DeleteOrUpdate = 1;

	DELETE FROM Orders
	WHERE courierID = @OldID;
END;
```

3. If the user enters ```@OldID``` and ```@WantToDeleteFromOrders``` 

>NOTE: ```@WantToDeleteFromOrders``` is a ```BIT``` variable, which indicates whether the user wants to ```DELETE``` some of the records from ```dbo.Orders``` (like in the example above) **OR** ```UPDATE``` ```dbo.Orders``` by setting a new value to the ```FOREIGN KEY``` (```courierID``` in the given example), which is linked to a specific table. ```@WantToDeleteFromOrders``` is set to 1 (```DELETE```) by default

```sql
EXEC dbo.delete_couriers 2, 0;
```

In this case, the procedure will ```UPDATE``` the records from ```dbo.Orders``` where the ```FOREIGN KEY``` (```courierID``` in the given example) linked to a specific table is equal to ```@OldID``` and set that ```FOREIGN KEY``` to ```NULL```
```sql
ELSE IF EXISTS (SELECT * FROM Couriers WHERE ID = @NewID)
		UPDATE Orders
		SET courierID = @NewID
		WHERE courierID = @OldID;
```

>NOTE: ```@NewID``` is the ```ID``` which the new value of the ```FOREIGN KEY``` is set to. It is ```NULL``` by default 

4. If the user enters ```@OldID```, ```@WantToDeleteFromOrders```, and a valid ```@NewID``` 

>NOTE: ```@NewID``` is considered as valid when a specific parent table (```dbo.Couriers``` in the given example) which has ```ID``` equal to ```@NewID```. In all other cases ```@NewID``` is invalid.

```sql
EXEC dbo.delete_couriers 3, 0, 4;
```

In this case, the procedure will ```UPDATE``` the records from ```dbo.Orders``` where the ```FOREIGN KEY``` (```courierID``` in the given example) linked to a specific table is equal to ```@OldID``` and set that ```FOREIGN KEY``` to ```@NewID```

The code is the same as in 3.
```sql
ELSE IF EXISTS (SELECT * FROM Couriers WHERE ID = @NewID)
		UPDATE Orders
		SET courierID = @NewID
		WHERE courierID = @OldID;
```

5. If the user enters ```@OldID```, ```@WantToDeleteFromOrders```, and a invalid ```@NewID```

```sql
EXEC dbo.delete_couriers 3, 0, 4;
```

In this case, the procedure will ```UPDATE``` the records from ```dbo.Orders``` where the ```FOREIGN KEY``` (```courierID``` in the given example) that is linked to a specific table is equal to ```@OldID``` and set that ```FOREIGN KEY``` to ```NULL```

```sql
ELSE
BEGIN
	SET @NewID = NULL;

	UPDATE Orders
	SET courierID = NULL
	WHERE courierID = @OldID;
END;
```

### *Type 2*: Delete procedure for ```dbo.Orders```

The delete procedure ```dbo.delete_orders``` deletes a record from ```dbo.Orders``` by a given ```@OldID``` (unless the ```@OldID``` is invalid; in that case, the procedure will ```PRINT``` a 'No such order exists' message and exit without making any alterations to ```dbo.Orders```)

```sql
CREATE OR ALTER PROC delete_orders @OldID INT
AS
BEGIN
	IF NOT EXISTS (SELECT * FROM Orders WHERE ID = @OldID)
	BEGIN
		PRINT 'No such order exists';
		RETURN;
	END;
	
	DELETE FROM Orders
	WHERE ID = @OldID;

	PRINT 'Deleted order with ID = ' + CAST(@OldID AS VARCHAR);
END;
```

Example:

```sql
EXEC dbo.delete_orders 1;
```

## Conclusion


[^1]: 1NF - First Normal Form

[^2]: 3NF - Third Normal Form

[^3]: Type - Type of Service

[^4]: BGN - Bulgarian Lev

[^crud]: 