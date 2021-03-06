# Data Engineering Technical Assessment
Completed Aug 6, 2021

---

### Table of Content

- [ETL Exercise](#etl-exercise)
- [Sample Client Email](#sample-client-email)
- [Analytics Exercise](#analytics-exercise)
- [Email Segmentation Exercise](#email-segmentation-exercise)
- [Proofing Exercise](#proofing-exercise)

---

## ETL Exercise

### Resources Used
- Postgres
- pgAdmin
- QuickDatabaseDiagrams.com

#### 1. Created Database Diagram
![Alt text](https://github.com/AnonApplicant/Assessment/blob/17b39860bb088dc9e088cf5f4e827b72238fb00f/ETL_Quick_Database_Drawing.png)

#### 2. Created Tables Using Provided Files
- [View easier to read SQL Queries](https://github.com/AnonApplicant/Assessment/blob/46e71ca437f7548224420dabb07b0cc768f1175f/sql_queries.sql)

`-- Create cons table`

`CREATE TABLE cons ( cons_id INT, prefix VARCHAR(4), firstname VARCHAR(20), middlename VARCHAR(20), lastname VARCHAR(20), suffix VARCHAR(5), salutation VARCHAR(20), gender VARCHAR(1), birth_dt DATE, title VARCHAR(25), employer VARCHAR(20), occupation VARCHAR(20), income FLOAT, source VARCHAR(20), subsource VARCHAR(20), userid VARCHAR(5) NOT NULL, password VARCHAR(20) NOT NULL, is_validated BOOLEAN NOT NULL, is_banned BOOLEAN NOT NULL, change_password_next_login BOOLEAN NOT NULL, consent_type_id VARCHAR(5) NOT NULL, create_dt TIMESTAMP NOT NULL, create_app INT NOT NULL, create_user VARCHAR(5) NOT NULL, modified_dt TIMESTAMP NOT NULL, modified_app VARCHAR(5) NOT NULL, modified_user VARCHAR(5) NOT NULL, status BOOLEAN NOT NULL, note VARCHAR(50), PRIMARY KEY (cons_id) );`

`-- Create cons_email table`

`CREATE TABLE cons_email ( cons_email_id VARCHAR(10) NOT NULL, cons_id INT NOT NULL, cons_email_type_id VARCHAR(10) NOT NULL, is_primary BOOLEAN NOT NULL, email VARCHAR(50) NOT NULL, canonical_local_part VARCHAR(40), domain VARCHAR(30) NOT NULL, double_validation VARCHAR(30), create_dt DATE NOT NULL, create_app VARCHAR(30) NOT NULL, create_user VARCHAR(30) NOT NULL, modified_dt DATE NOT NULL, modified_app VARCHAR(30) NOT NULL, modified_user VARCHAR(30) NOT NULL, status BOOLEAN NOT NULL, note VARCHAR(100), FOREIGN KEY (cons_id) REFERENCES cons (cons_id), PRIMARY KEY (cons_email_id) );`

`-- Create cons_email_chapter_subscription table`

`CREATE TABLE cons_email_chapter_subscription ( cons_email_chapter_subscription_id INT NOT NULL, cons_email_id VARCHAR(10) NOT NULL, chapter_id INT NOT NULL, isunsub BOOLEAN NOT NULL, unsub_dt TIMESTAMP NOT NULL, modified_dt TIMESTAMP NOT NULL, FOREIGN KEY (cons_email_id) REFERENCES cons_email (cons_email_id), PRIMARY KEY (cons_email_chapter_subscription_id) );`

#### 3. Performed Inner Joins
- [View easier to read SQL Queries](https://github.com/AnonApplicant/Assessment/blob/46e71ca437f7548224420dabb07b0cc768f1175f/sql_queries.sql)

`-- Create pre_people table`

`CREATE TABLE pre_people AS
SELECT cons.cons_id, cons.subsource, cons_email.cons_email_id, email, cons_email.is_primary, 
cons.create_dt AS created_dt, cons.modified_dt AS updated_dt, cons_email_chapter_subscription.isunsub  
FROM cons
INNER JOIN cons_email
ON cons.cons_id = cons_email.cons_id
INNER JOIN cons_email_chapter_subscription
ON cons_email.cons_email_id = cons_email_chapter_subscription.cons_email_id AND chapter_id = 1
WHERE cons_email.is_primary = true`

#### 4. Created People Table

`-- Create people table`

`CREATE TABLE people AS
SELECT email, subsource AS code, isunsub AS is_unsub, created_dt, updated_dt FROM pre_people;`


#### 5. Created Aquisition Facts

`-- Create acquisition_facts`

`SELECT DATE(created_dt) AS acquisition_date, COUNT(distinct email) 
FROM people
GROUP BY acquisition_date
ORDER BY acquisition_date desc;`

### ETL Deliverable I: People Table
- Download [people.csv](https://github.com/AnonApplicant/Assessment/blob/0359ad6e97d2076b46ce13196d139a5722fb68ce/people.csv) 

_(Preview below)_

![people table preview](https://github.com/AnonApplicant/Assessment/blob/720d7d0604b113227c3428fc4251e17aff983b15/people_table_preview.png)

### ETL Deliverable 2: Acquisition Facts
- View [aquisition_facts.csv](https://github.com/AnonApplicant/Assessment/blob/ecd0d546940ca67630e3432def5b62ccc5dc6ea2/acquisition_facts.csv)

_(Preview below)_

![aquisition facts preview](https://github.com/AnonApplicant/Assessment/blob/5cea7f58a61afb004adb7397734801ab83ba3904/acquisition_facts_preview.png)

---

## Sample Client Email
- View [Sample Email]()

## Analytics Exercise
- View completed [Analytics Exercise](https://github.com/AnonApplicant/Assessment/blob/4a7db16994f600b49ca3141d06b1e9a44db81e25/Analytics%20Exercise%20Results.pdf)
- Download [Analytics Exercise Scratch Work](https://github.com/AnonApplicant/Assessment/blob/80700a6c8a964936190c395630c69a57e6a80be1/Analytics%20Exercise_Work.xlsx)

## Email Segmentation Exercise
- View completed [Segmentation Quality Assurance Exercise.pdf](https://github.com/AnonApplicant/Assessment/blob/741d39492da79707cc2466cbfc7491221bc89dc2/Segmentation%20Quality%20Assurance%20Exercise.pdf)

## Proofing Exercise
- Download completed [General Proofing Exercise.docx](https://github.com/AnonApplicant/Assessment/blob/24f61a4524847a129d216f1a94ca9543cf1b53fa/General%20Proofing%20Exercise.docx)
