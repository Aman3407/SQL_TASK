# SQL_TASK

Problem Statement 
![image](https://github.com/Aman3407/SQL_TASK/assets/174441737/6873480b-c673-4287-8f65-67b003d4b655)

```
Create table CUSTOMER
(
customer_id int primary key,
name varchar(50) not null,
address varchar(50) not null
);

create table PRODUCT
(
product_id int primary key,
name varchar(50) not null,
price float not null
);

create table ORDERS
(
order_id int primary key,
order_date date not null,
customer_id  int,
foreign key (customer_id) references customer(customer_id)
);

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

<img width="240" alt="image" src="https://github.com/Aman3407/SQL_TASK/assets/174441737/e1be0282-85f4-40b7-8e04-394d1f38f7f9">
<img width="243" alt="image" src="https://github.com/Aman3407/SQL_TASK/assets/174441737/2de72391-e67c-4827-a6ff-7e4581560d45">
<img width="257" alt="image" src="https://github.com/Aman3407/SQL_TASK/assets/174441737/8805a48b-324d-4fc6-9160-ec31ffc8a8fd">
<img width="256" alt="image" src="https://github.com/Aman3407/SQL_TASK/assets/174441737/a5730091-d787-4a1c-bfaa-1b402bed72e9">
<br>
<img width="591" alt="image" src="https://github.com/Aman3407/SQL_TASK/assets/174441737/d0af44ee-51eb-4d3f-a7bd-acc31d62573d">


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
<img width="803" alt="image" src="https://github.com/Aman3407/SQL_TASK/assets/174441737/fcf61fa5-a5f8-48ee-8ab8-7f793f2dd650">
<img width="779" alt="image" src="https://github.com/Aman3407/SQL_TASK/assets/174441737/c3708907-8014-457e-ada8-863e870ca3c4">
<img width="770" alt="image" src="https://github.com/Aman3407/SQL_TASK/assets/174441737/8e04cc3f-552b-4020-bbb7-1b0df28c64f2">





