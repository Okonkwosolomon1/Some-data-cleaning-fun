[carchoice.csv](https://github.com/Okonkwosolomon1/Some-data-cleaning-fun/files/14067802/carchoice.csv)## A LITTLE BIT OF DATA CLEANING 
### The dataset

[carchoice dataset.docx](https://github.com/Okonkwosolomon1/Some-data-cleaning-fun/files/14067889/carchoice.dataset.docx)

```sql
CREATE DATABASE da_week1_assignment;
```
```sql
USE da_week1_assignment;
```
#### To check if the table imported correctly
```sql
SELECT *
FROM carchoice;
```
#### Next to check the datatypes:
```sql
DESCRIBE carchoice;
```
[carchoice dataset.csv](https://github.com/Okonkwosolomon1/Some-data-cleaning-fun/files/14067540/carchoice.dataset.csv)

#### average price came as text instead of BIGNIT. Let's change it

```sql
ALTER TABLE carchoice
MODIFY average_price VARCHAR(10);
```
#### Several attempts were made to modify the colun: "average_price" to
#### Integer or Big Integer but failed. Reason is because of the dollar sign($)
#### present in the column entries. So I changed it to Varchar.
#### Next I will remove the $ and re-modify the column as BigInteger(BIGINT)

#### Since we want to deal with the entries of the column and not the column itself,
#### we will use the UPDATE TABLE function rather than ALTER TABLE.

#### First, however, we need to set SQL safe mode to zero before updating the table else it wont update

```sql
SET sql_safe_updates = 0;
```
#### This is to set the sql safe mode to OFF position so we can update the table carchoice
#### Next we change the datatype of average_price column to biint;
```sql
ALTER TABLE carchoice
MODIFY COLUMN average_price BIGINT;
```
```sql
UPDATE carchoice
SET average_price = REPLACE(average_price,'$','');
```
#### Let's check if the change took place:
```sql
SELECT average_price
FROM carchoice;
```
#### It did. We notice another problem though. There's a comma in the midst of the "numbers". 
#### To remove it, we use same approach as above
```sql
UPDATE carchoice
SET average_price = REPLACE(average_price,',','');
```
#### Let's check if the change happened.
```sql
SELECT average_price
FROM carchoice;
```
#### It did. recall we replaced the ',' with empty space''. Sometimes SQL (and other softwares)
#### sees this as a letter or number present and not just an 'empty space'. To be sure, let's check
#### the length of the characters in the column entries. we expect 5 as an answer since there are 5
#### digits in numbers like 22000. But if we see 6 then there's a problem
```sql
SELECT LENGTH(average_price)
FROM carchoice;
```
#### As suspected, we are seeing 6 instead of 5. So now, we have an added work to do. That is to remove the "empty space"
```sql
UPDATE carchoice
SET average_price = TRIM(average_price);
```
#### Yay! we trimmed the column then checked the result by re-running the "SELECT average_price..." code and 
#### we now have 5 digits as we wanted. We can now proceed with analysing the data presented

### Question 1: Count the Price Entries
```sql
SELECT COUNT(average_price) AS num_price_entries
FROM carchoice;
```
#### Ans =25

### Question 2: Count Car Models
```sql
SELECT COUNT(car_model)
FROM carchoice;
```
#### Ans = 25

### Question 3: Count Cars by Toyota
```sql
SELECT SUM(CASE WHEN car_brand ='Toyota' THEN 1 END) AS num_toyota
FROM carchoice;
```
### Ans = 8

### Question 4: Average Price of Cars by Honda
#### Method1
```sql
SELECT round(AVG(CASE WHEN car_brand = 'Honda' THEN average_price END),2) AS avg_price_honda
FROM carchoice;
```
#### Method 2:
```sql
SELECT round(AVG(average_price),2)
FROM carchoice
WHERE car_brand = 'Honda';
```
### Supose we want to add the "$"
```sql
SELECT CONCAT('$',round(AVG(CASE WHEN car_brand = 'Honda' THEN average_price END),2)) AS avg_price_honda
FROM carchoice;
```
#### Ans $30285.71 (to 2 d.p)

### Question 5: Total Cost of Cars Owned by Michael Smith
```sql
SELECT SUM(average_price)
FROM carchoice
WHERE buyer_names = 'Michael Smith';
```
#### Ans 97000
### To add dollar sign '$' to the answer:
```sql
SELECT CONCAT('$',SUM(average_price)) AS total_amount_michael
FROM carchoice
WHERE buyer_names = 'Michael Smith';
```

### Question 6: Average Price of Ford Cars owned by Brian
```sql
SELECT CONCAT('$',AVG(average_price)) AS avg_amt_ford_Brian
FROM carchoice
WHERE car_brand = 'Ford'
AND buyer_names LIKE 'Brian%';
```
#### Ans $32000

##### Remember to turn the safe mode back on
```sql
SET sql_safe_updates = 1;
```

### END


