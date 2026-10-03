CREATE DATABASE FoodDelivery
USE FoodDelivery

--Price mənfi ola bilməz.
--Stock mənfi ola bilməz.
--Rating 1–5 arasında olmalıdır.
--Email təkrarlana bilməz.
--Quantity ən azı 1 olmalıdır.
--OrderDate gələcək tarix ola bilməz.
--Status üçün default olaraq Pending ver.

CREATE TABLE Customers(
Id INT PRIMARY KEY IDENTITY,
FirstName NVARCHAR (50) NOT NULL,
LastName NVARCHAR (50) NOT NULL,
Phone VARCHAR (50),
Email VARCHAR (150) NOT NULL UNIQUE,
Address VARCHAR (100) NOT NULL
)

CREATE TABLE Restaurants(
Id INT PRIMARY KEY IDENTITY,
Name VARCHAR (50) NOT NULL,
Address VARCHAR (100) NOT NULL,
Rating INT CHECK (Rating >=1 AND Rating <=5)
)

ALTER TABLE Restaurants 
ADD DistanceFromCenter DECIMAL (5,2)

CREATE TABLE DeliveryZones(
Id INT PRIMARY KEY IDENTITY,
ZoneName VARCHAR (50) NOT NULL,
MinDistance DECIMAL (4, 2) NOT NULL,
MaxDistance INT NOT NULL,
DeliveryFee INT NOT NULL
)

CREATE TABLE Categories(
Id INT PRIMARY KEY IDENTITY,
Name VARCHAR (50) NOT NULL
)

CREATE TABLE Products(
Id INT PRIMARY KEY IDENTITY,
Name VARCHAR (50) NOT NULL,
Price DECIMAL (10,2) NOT NULL CHECK (Price > 0),
Stock INT CHECK (Stock >= 0),
RestaurantId INT FOREIGN KEY REFERENCES Restaurants(Id),
CategoryId INT FOREIGN KEY REFERENCES Categories(Id)
)

CREATE TABLE Couirers(
Id INT PRIMARY KEY IDENTITY,
FirstName NVARCHAR (50) NOT NULL,
LastName NVARCHAR (50) NOT NULL,
Phone VARCHAR (50),
Salary DECIMAL (5,2) 
)

CREATE TABLE Orders(
Id INT PRIMARY KEY IDENTITY,
CustomerId INT FOREIGN KEY REFERENCES Customers(Id),
CouirerId INT FOREIGN KEY REFERENCES Couirers(Id),
OrderDate DATE CHECK (OrderDate <= GETDATE()),
Status VARCHAR(20) DEFAULT 'Pending',
TotalPrice DECIMAL (10,2) CHECK (TotalPrice >=0)
)

CREATE TABLE OrderItems(
Id INT PRIMARY KEY IDENTITY,
OrderId INT FOREIGN KEY REFERENCES Orders(Id),
ProductId INT FOREIGN KEY REFERENCES Products(Id),
Quantity INT CHECK (Quantity > 0)
)

-- 8 Customers
INSERT INTO Customers (FirstName, LastName, Phone, Email, Address)
VALUES
('Ali', 'Mammadov', '0501112233', 'ali@gmail.com', 'Baku, Yasamal'),
('Leyla', 'Hasanova', '0512223344', 'leyla@gmail.com', 'Baku, Narimanov'),
('Murad', 'Aliyev', '0553334455', 'murad@gmail.com', 'Baku, Genclik'),
('Aysel', 'Huseynova', '0704445566', 'aysel@gmail.com', 'Baku, Nizami'),
('Kamran', 'Rahimov', '0775556677', 'kamran@gmail.com', 'Baku, Sabunchu'),
('Nigar', 'Karimova', '0506667788', 'nigar@gmail.com', 'Baku, Khatai'),
('Elvin', 'Jafarov', '0517778899', 'elvin@gmail.com', 'Baku, Bayil'),
('Zehra', 'Ismayilova', '0558889900', 'zehra@gmail.com', 'Baku, Binagadi');


-- 5 Restaurants
INSERT INTO Restaurants (Name, Address, Rating)
VALUES
('Pizza House', 'Baku, Yasamal', 4.7),
('Burger Point', 'Baku, Narimanov', 4.5),
('Sushi World', 'Baku, Genclik', 4.8),
('Chicken Time', 'Baku, Nizami', 4.2),
('Pasta House', 'Baku, Khatai', 4.4);

-- 6 Categories
INSERT INTO Categories (Name)
VALUES
('Pizza'),
('Burger'),
('Sushi'),
('Chicken'),
('Pasta'),
('Drinks');


-- 15 Products
INSERT INTO Products (Name, Price, Stock, RestaurantId, CategoryId)
VALUES
('Margherita Pizza', 12.00, 25, 1, 1),
('Pepperoni Pizza', 15.00, 20, 1, 1),
('Cheese Pizza', 13.50, 18, 1, 1),

('Classic Burger', 9.00, 30, 2, 2),
('Double Burger', 13.00, 22, 2, 2),
('Cheese Burger', 11.00, 15, 2, 2),

('California Roll', 18.00, 12, 3, 3),
('Salmon Sushi', 22.00, 10, 3, 3),
('Tuna Roll', 19.00, 14, 3, 3),

('Fried Chicken', 10.00, 25, 4, 4),
('Chicken Wings', 8.50, 35, 4, 4),
('Chicken Burger', 11.50, 20, 4, 4),

('Carbonara', 16.00, 15, 5, 5),
('Bolognese', 17.00, 13, 5, 5),
('Penne Alfredo', 15.00, 16, 5, 5);


-- 6 Couriers
INSERT INTO Couirers (FirstName, LastName, Phone, Salary)
VALUES
('Rashad', 'Aliyev', '0501234567', 120),
('Tural', 'Mammadov', '0512345678', 135),
('Orkhan', 'Hasanov', '0553456789', 150),
('Samir', 'Huseynov', '0704567890', 110),
('Emin', 'Rahimov', '0775678901', 140),
('Nijat', 'Karimov', '0506789012', 125)


-- 15 Orders
INSERT INTO Orders (CustomerId, CouirerId, OrderDate, Status, TotalPrice)
VALUES
(1, 3, '2026-09-20 12:30', 'Delivered', 27.00),
(2, 4, '2026-09-20 13:15', 'Delivered', 18.00),
(3, 5, '2026-09-21 14:00', 'Delivered', 40.00),
(4, 6, '2026-09-21 18:30', 'Delivered', 25.00),
(5, 7, '2026-09-22 19:00', 'Delivered', 32.00),
(6, 8, '2026-09-22 20:15', 'Pending', 22.00),
(7, 3, '2026-09-23 12:00', 'Delivered', 26.00),
(8, 4, '2026-09-23 13:45', 'Preparing', 18.00),
(1, 5, '2026-09-24 15:00', 'Delivered', 33.00),
(2, 6, '2026-09-24 17:30', 'Cancelled', 17.00),
(3, 7, '2026-09-25 18:00', 'Delivered', 44.00),
(4, 8, '2026-09-25 19:15', 'Pending', 30.00),
(5, 3, '2026-09-26 20:00', 'Delivered', 24.00),
(6, 4, '2026-09-27 12:45', 'Preparing', 15.00),
(7, 5, '2026-09-27 14:30', 'Delivered', 34.00);


-- 25 OrderItems
INSERT INTO OrderItems (OrderId, ProductId, Quantity)
VALUES
(3, 1, 1),
(3, 2, 1),

(4, 4, 2),

(5, 7, 1),
(5, 8, 1),

(6, 10, 2),
(6, 11, 1),

(7, 13, 1),
(7, 14, 1),

(8, 8, 1),

(9, 5, 2),

(10, 6, 1),
(10, 4, 1),

(11, 2, 1),
(11, 3, 1),

(12, 14, 1),

(13, 8, 2),

(14, 10, 1),
(14, 12, 1),

(15, 13, 1),
(15, 1, 1),

(16, 15, 1),

(17, 7, 1),
(17, 9, 1);

SELECT
Products.Name AS ProductName,
Products.Price AS Price,
Restaurants.Name AS RestaurantName
FROM Products 
JOIN Restaurants 
ON Products.RestaurantId = RestaurantId

SELECT
Products.Name AS ProductName,
Categories.Name AS CategoryName,
Products.Price AS Price
FROM Categories
JOIN Products
ON Products.CategoryId = Categories.Id

SELECT
Orders.Id AS OrderId,
CONCAT (Customers.FirstName, ' ' ,Customers.LastName) CustomersName,
Orders.OrderDate AS OrderDate,
Orders.Status AS OrderStatus,
Orders.TotalPrice AS TotalPrice
FROM Orders
JOIN Customers
ON Orders.CustomerId = Customers.Id

SELECT
CONCAT (Customers.FirstName, ' ' ,Customers.LastName) CustomersName,
Orders.Id AS OrderId,
Products.Name AS ProductName,
OrderItems.Quantity
FROM Customers
JOIN Orders
ON Orders.CustomerId = Customers.Id
JOIN OrderItems
On OrderItems.OrderId = Orders.Id
JOIN Products
ON OrderItems.ProductId = Products.Id

SELECT 
Products.Name AS ProductName,
Restaurants.Name AS RestaurantName
FROM Restaurants 
LEFT JOIN Products 
ON Restaurants.Id = RestaurantId

SELECT
Restaurants.Name AS RestaurantName,
Restaurants.DistanceFromCenter,
DeliveryZones.ZoneName,
DeliveryZones.DeliveryFee
FROM Restaurants 
JOIN DeliveryZones 
ON Restaurants.DistanceFromCenter >= DeliveryZones.MinDistance
AND Restaurants.DistanceFromCenter <= DeliveryZones.MaxDistance

SELECT
Restaurants.Name AS RestaurantName,
DeliveryZones.ZoneName
FROM Restaurants
CROSS JOIN DeliveryZones

SELECT
Restaurants.Name AS RestaurantName,
Products.Name AS ProductName,
Categories.Name AS CategoryName,
Products.Price AS Price,
DeliveryZones.ZoneName AS ZoneName,
DeliveryZones.DeliveryFee AS DeliveryFee
FROM Restaurants 
JOIN Products
ON Products.RestaurantId = Restaurants.Id
JOIN Categories
ON Products.CategoryId = Categories.Id
JOIN DeliveryZones
ON Restaurants.DistanceFromCenter >= DeliveryZones.MinDistance
AND Restaurants.DistanceFromCenter <= DeliveryZones.MaxDistance