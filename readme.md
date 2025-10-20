E-Commerce Database
A comprehensive MySQL database schema for managing an e-commerce platform with customers, products, orders, and order items.
Database Overview
The e_commerce database is designed to handle core e-commerce operations including customer management, product catalog, order processing, and order item tracking.
Database Schema
Tables
1. Customers
Stores customer information for the e-commerce platform.
Columns:

customer_id (INT, Primary Key, Auto Increment) - Unique identifier for each customer
first_name (VARCHAR(100), NOT NULL) - Customer's first name
last_name (VARCHAR(100), NOT NULL) - Customer's last name
email (VARCHAR(150), UNIQUE, NOT NULL) - Customer's email address
phone_number (VARCHAR(20), NULLABLE) - Customer's phone number

Constraints:

Email must be unique across all customers
First name and last name are required fields

2. Products
Contains the product catalog with pricing information.
Columns:

product_id (INT, Primary Key, Auto Increment) - Unique identifier for each product
product_name (VARCHAR(255), UNIQUE, NOT NULL) - Product name
description (TEXT, NULLABLE) - Detailed product description
price (DECIMAL(10,2), NOT NULL) - Product price

Constraints:

Product name must be unique
Price must be greater than or equal to 0
Price is required for all products

3. Orders
Tracks customer orders with shipping and total information.
Columns:

order_id (INT, Primary Key, Auto Increment) - Unique identifier for each order
customer_id (INT, Foreign Key, NOT NULL) - References the customer who placed the order
order_date (DATETIME, NOT NULL) - Date and time when the order was placed
total_amount (DECIMAL(10,2), NOT NULL) - Total order amount
shipping_address (VARCHAR(255), NOT NULL) - Delivery address for the order

Constraints:

Foreign key relationship with Customers table
ON DELETE CASCADE: Deleting a customer will delete all their orders
ON UPDATE CASCADE: Updating a customer_id will update all related orders

4. Order_Items
Junction table linking orders with products and storing quantity information.
Columns:

order_id (INT, Foreign Key, NOT NULL) - References the order
product_id (INT, Foreign Key, NOT NULL) - References the product
quantity (INT, NOT NULL) - Number of units ordered
unit_price (DECIMAL(10,2), NOT NULL) - Price per unit at the time of order

Constraints:

Composite primary key (order_id, product_id)
Foreign key relationship with Orders table (ON DELETE RESTRICT, ON UPDATE CASCADE)
Foreign key relationship with Products table (ON DELETE RESTRICT, ON UPDATE CASCADE)
Quantity must be greater than 0
DELETE RESTRICT prevents deletion of orders or products that have associated order items

Relationships
CUSTOMERS (1) ----< (M) ORDERS (1) ----< (M) ORDER_ITEMS (M) >---- (1) PRODUCTS

One customer can place multiple orders
One order can contain multiple order items
One product can appear in multiple order items
Order_Items creates a many-to-many relationship between Orders and Products

Sample Data
The database includes sample data for testing:

5 Customers: Alice Johnson, Bob Smith, Charlie Brown, Diana Prince, and Ethan Hunt
5 Products: Laptop Pro X1, Wireless Mouse M300, Mechanical Keyboard K90, 4K Monitor UltraView, and USB-C Hub 7-in-1
4 Orders: Various orders placed by different customers
Multiple Order Items: Items distributed across the orders

Installation

Create the database:

sqlCREATE DATABASE e_commerce;

Run the table creation scripts in the following order:

Customers table
Products table
Orders table
Order_Items table


Insert sample data using the provided INSERT statements

Usage Examples
Query all orders for a specific customer
sqlSELECT o.order_id, o.order_date, o.total_amount, o.shipping_address
FROM e_commerce.Orders o
WHERE o.customer_id = 1;
Get order details with products
sqlSELECT o.order_id, c.first_name, c.last_name, p.product_name, oi.quantity, oi.unit_price
FROM e_commerce.Orders o
JOIN e_commerce.Customers c ON o.customer_id = c.customer_id
JOIN e_commerce.Order_Items oi ON o.order_id = oi.order_id
JOIN e_commerce.Products p ON oi.product_id = p.product_id
WHERE o.order_id = 1;
Calculate total revenue
sqlSELECT SUM(total_amount) as total_revenue
FROM e_commerce.Orders;
Key Features

Data Integrity: Foreign key constraints ensure referential integrity
Cascade Operations: Customer deletion automatically removes associated orders
Restrict Operations: Orders and products cannot be deleted if they have associated order items
Validation: CHECK constraints ensure positive prices and quantities
Unique Constraints: Prevent duplicate emails and product names

Notes

The unit_price in Order_Items preserves the historical price at the time of purchase, even if the product price changes later
Phone numbers are optional for customers
Product descriptions are optional
All monetary values use DECIMAL(10,2) for precision

Future Enhancements
Potential improvements to consider:

Add inventory management table
Implement order status tracking
Add payment information table
Include product categories
Add customer addresses table for multiple shipping addresses
Implement reviews and ratings system
