# SQL Task

## Overview

This document presents the SQL task that involves creating and managing tables to store customer, product, order, and order details information. Additionally, it demonstrates how to work with unlabeled image predictions by creating and manipulating a corresponding table. The goal is to handle and query data efficiently using SQL.

## Table Definitions

The following SQL statements create the necessary tables for managing customer, product, and order information:

Problem Statement 
![image](https://github.com/Aman3407/SQL_TASK/assets/174441737/6873480b-c673-4287-8f65-67b003d4b655)

### Customer Table
```
Create table CUSTOMER
(
customer_id int primary key,
name varchar(50) not null,
address varchar(50) not null
);
```
<img width="240" alt="image" src="https://github.com/Aman3407/SQL_TASK/assets/174441737/e1be0282-85f4-40b7-8e04-394d1f38f7f9">

### Product Table

```
create table PRODUCT
(
product_id int primary key,
name varchar(50) not null,
price float not null
);
```
<img width="243" alt="image" src="https://github.com/Aman3407/SQL_TASK/assets/174441737/2de72391-e67c-4827-a6ff-7e4581560d45">

### Orders Table

```
create table ORDERS
(
order_id int primary key,
order_date date not null,
customer_id  int,
foreign key (customer_id) references customer(customer_id)
);
```
<img width="257" alt="image" src="https://github.com/Aman3407/SQL_TASK/assets/174441737/8805a48b-324d-4fc6-9160-ec31ffc8a8fd">

### OrderDetails Table
```
create table ORDER_DETAILS
(
order_id int,
product_id int,
quantity int,
Primary key (order_id,product_id),
foreign key (product_id) references product(product_id),
foreign key (order_id) REFERENCES orders(order_id)
);
```
<img width="256" alt="image" src="https://github.com/Aman3407/SQL_TASK/assets/174441737/a5730091-d787-4a1c-bfaa-1b402bed72e9">

## ER DIAGRAM
![image](https://github.com/Aman3407/SQL_TASK/assets/174441737/15fb5f40-f50c-408a-8366-e78d034de7ab)

<br>

## TASK 2
<img width="591" alt="image" src="https://github.com/Aman3407/SQL_TASK/assets/174441737/d0af44ee-51eb-4d3f-a7bd-acc31d62573d">


## Sample Data Insertion

### The following SQL statements insert sample data into the unlabeled_image_predictions table:

```
CREATE TABLE IF NOT EXISTS unlabeled_image_predictions (
    image_id int,
    score float
);

INSERT INTO unlabeled_image_predictions (image_id, score) VALUES
(18281, 0.31491),
(17051, 0.98921),
(1146, 0.56161),
(594, 0.76701),
(232, 0.15981),
(5241, 0.98761),
(306, 0.6487),
(11321, 0.88231),
(19061, 0.83941),
(12721, 0.97781),
(1616, 0.10031),
(1161, 0.71131),
(1715, 0.89211),
(11091, 0.1151),
(1424, 0.77901),
(1609, 0.52411),
(1631, 0.25521),
(12761, 0.26721),
(17011, 0.0758),
(554, 0.4418),
(998, 0.0379),
(809, 0.1058),
(1219, 0.71431),
(402, 0.7655),
(3631, 0.26611),
(16241, 0.82701),
(1640, 0.8790),
(913, 0.2421),
(1439, 0.33871),
(1464, 0.36741),
(1405, 0.69291),
(19861, 0.89311),
(344, 0.37611),
(847, 0.48891),
(482, 0.50231),
(1823, 0.33611),
(16171, 0.02181),
(1471, 0.00721),
(18671, 0.4050),
(1961, 0.44981),
(126, 0.35641),
(943, 0.0452),
(1115, 0.53091),
(1417, 0.7168),
(1706, 0.96491),
(1166, 0.25071),
(1991, 0.41911),
(1465, 0.0895),
(153, 0.8169),
(971, 0.9871);
```

## Query Solution : 
```
with asce as(
  select ROW_NUMBER() over(order by score desc) as rno,image_id,score as weak_label from
  unlabeled_image_predictions
),
 desce as(
  select ROW_NUMBER() over(order by score) as
  rno,image_id,score as weak_label from
  unlabeled_image_predictions
)
,final_table as (select image_id, ROUND(weak_label) from asce where rno%3=1 and rno<=30000 AND weak_label > 0.5
union
select image_id,ROUND(weak_label) from desce where rno%3=1 and rno<=30000 AND weak_label <=0.5)
select * from final_table order by image_id;
```

## OUTPUT
<img width="803" alt="image" src="https://github.com/Aman3407/SQL_TASK/assets/174441737/fcf61fa5-a5f8-48ee-8ab8-7f793f2dd650">
<img width="779" alt="image" src="https://github.com/Aman3407/SQL_TASK/assets/174441737/c3708907-8014-457e-ada8-863e870ca3c4">
<img width="770" alt="image" src="https://github.com/Aman3407/SQL_TASK/assets/174441737/8e04cc3f-552b-4020-bbb7-1b0df28c64f2">


# TASK 2

## DESCRIPTION

Your task is to prepare a list of cities with the date of last reservation made in the city and a main photo (photos[0]) of the most popular (by number of bookings) hotel in this city.

Sort results in ascending order by city. If many hotels have the same amount of bookings sort them by ID (ascending order).
Remember that the query will also be run of different datasets.

```sql
CREATE TABLE city
(
	id INT PRIMARY KEY,
	name VARCHAR(50) NOT NULL
);

CREATE TABLE hotel
(
	id INT PRIMARY KEY,
	city_id INT NOT NULL REFERENCES city,
	name VARCHAR(50) NOT NULL,
	day_price NUMERIC(8, 2) NOT NULL,
	photos JSONB DEFAULT '[]'
);


CREATE TABLE booking
(
	id int PRIMARY KEY,
	hotel_id INT NOT NULL REFERENCES hotel,
	booking_date DATE NOT NULL,
	start_date DATE NOT NULL,
	end_date DATE NOT NULL
);

INSERT INTO city (id, name) VALUES
(1, 'Barcelona'),
(2, 'Roma'),
(3, 'Paris'),
(4, 'New York'),
(5, 'Tokyo'),
(6, 'Sydney');

INSERT INTO hotel (id, city_id, name, day_price, photos) VALUES
(1, 1, 'The New View', 67.00, '["1-1.jpg", "1-2.jpg"]'),
(2, 1, 'The Upper House', 99.00, '["2-1.jpg", "2-2.jpg"]'),
(3, 2, 'Ace Hotel', 90.00, '["3-1.jpg", "3-2.jpg"]'),
(4, 2, 'Hotel Diva', 111.00, '["4-1.jpg", "4-2.jpg"]'),
(5, 3, 'Hotel Triton', 105.00, '["5-1.jpg", "5-2.jpg"]'),
(6, 4, 'Grand Palace', 150.00, '["6-1.jpg", "6-2.jpg"]'),
(7, 5, 'Tokyo Tower Inn', 200.00, '["7-1.jpg", "7-2.jpg"]'),
(8, 6, 'Sydney Suites', 180.00, '["8-1.jpg", "8-2.jpg"]'),
(9, 4, 'New York Inn', 120.00, '["9-1.jpg", "9-2.jpg"]'),
(10, 5, 'Sakura Hotel', 220.00, '["10-1.jpg", "10-2.jpg"]');

INSERT INTO booking (id, hotel_id, booking_date, start_date, end_date) VALUES
(1, 1, '2024-06-10', '2024-07-01', '2024-07-05'),
(2, 2, '2024-06-11', '2024-07-02', '2024-07-06'),
(3, 3, '2024-06-12', '2024-07-03', '2024-07-07'),
(4, 4, '2024-06-13', '2024-07-04', '2024-07-08'),
(5, 5, '2024-06-14', '2024-07-05', '2024-07-09'),
(6, 6, '2024-06-15', '2024-07-06', '2024-07-10'),
(7, 7, '2024-06-16', '2024-07-07', '2024-07-11'),
(8, 8, '2024-06-17', '2024-07-08', '2024-07-12'),
(9, 9, '2024-06-18', '2024-07-09', '2024-07-13'),
(10, 10, '2024-06-19', '2024-07-10', '2024-07-14'),
(11, 1, '2024-06-20', '2024-07-11', '2024-07-15'),
(12, 2, '2024-06-21', '2024-07-12', '2024-07-16'),
(13, 3, '2024-06-22', '2024-07-13', '2024-07-17'),
(14, 4, '2024-06-23', '2024-07-14', '2024-07-18'),
(15, 5, '2024-06-24', '2024-07-15', '2024-07-19'),
(16, 6, '2024-06-25', '2024-07-16', '2024-07-20'),
(17, 7, '2024-06-26', '2024-07-17', '2024-07-21'),
(18, 8, '2024-06-27', '2024-07-18', '2024-07-22'),
(19, 9, '2024-06-28', '2024-07-19', '2024-07-23'),
(20, 10, '2024-06-29', '2024-07-20', '2024-07-24');
```
## SOLUTION
```sql
WITH 
BookingCounts AS (
  SELECT h.city_id,
         h.id AS hotel_id,
         COUNT(b.id) AS number_of_bookings
    FROM hotel h
         JOIN booking b ON h.id = b.hotel_id
   GROUP BY h.city_id, h.id
),
MaxBookingCounts AS (
  SELECT bc.city_id,
         MAX(bc.number_of_bookings) AS max_booking_count
    FROM BookingCounts bc
   GROUP BY bc.city_id
),
MostBookedHotels AS (
  SELECT bc.city_id,
         bc.hotel_id,
         bc.number_of_bookings
    FROM BookingCounts bc
         JOIN MaxBookingCounts mbc ON mbc.city_id = bc.city_id
                               AND bc.number_of_bookings = mbc.max_booking_count
),
LastBookingByCity AS (
  SELECT h.city_id,
         MAX(b.booking_date) AS last_booking_date
    FROM booking b
         JOIN hotel h ON h.id = b.hotel_id
   GROUP BY h.city_id
),
HotelPhotos AS (
  SELECT h.id AS hotel_id,
         (h.photos->>0) AS photo
    FROM hotel h
),
FinalResult AS (
  SELECT c.name AS city_name,
         lb.last_booking_date,
         mbh.hotel_id,
         hp.photo
    FROM MostBookedHotels mbh
         JOIN city c ON mbh.city_id = c.id
         JOIN LastBookingByCity lb ON c.id = lb.city_id
         JOIN HotelPhotos hp ON mbh.hotel_id = hp.hotel_id
)
 
SELECT *
  FROM FinalResult
 ORDER BY city_name, hotel_id;
```
OUTPUT :-

<img width="706" alt="image" src="https://github.com/user-attachments/assets/a8cac39e-d19d-40dd-af93-ad136e90eafe">

