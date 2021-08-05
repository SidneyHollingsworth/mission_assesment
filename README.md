# ALS - Data Engineering Exercise

### Used SQL

#### Created tables with provided files

`-- Created cons table
CREATE TABLE cons (
	cons_id INT,
	prefix VARCHAR(4),
	firstname VARCHAR(20),
	middlename VARCHAR(20),
	lastname VARCHAR(20),
	suffix VARCHAR(5),
	salutation VARCHAR(20),
	gender VARCHAR(1),
	birth_dt DATE,
	title VARCHAR(25),
	employer VARCHAR(20),
	occupation VARCHAR(20),
	income FLOAT,
	source VARCHAR(20),
	subsource VARCHAR(20),
	userid VARCHAR(5) NOT NULL, 
	password VARCHAR(20) NOT NULL,
	is_validated BOOLEAN NOT NULL,
	is_banned BOOLEAN NOT NULL,
	change_password_next_login BOOLEAN NOT NULL,
	consent_type_id VARCHAR(5) NOT NULL,
	create_dt TIMESTAMP NOT NULL,
	create_app INT NOT NULL,
	create_user VARCHAR(5) NOT NULL,
	modified_dt TIMESTAMP NOT NULL,
	modified_app VARCHAR(5) NOT NULL,
	modified_user VARCHAR(5) NOT NULL,
	status BOOLEAN  NOT NULL,
	note VARCHAR(50), 
  PRIMARY KEY (cons_id)
);`

`-- Created cons_email table
CREATE TABLE cons_email (
	cons_email_id VARCHAR(10) NOT NULL,
	cons_id INT NOT NULL,
	cons_email_type_id VARCHAR(10) NOT NULL,
	is_primary BOOLEAN NOT NULL,
	email VARCHAR(50) NOT NULL,
	canonical_local_part VARCHAR(40),
	domain VARCHAR(30) NOT NULL,
	double_validation VARCHAR(30),
	create_dt DATE NOT NULL,
	create_app VARCHAR(30) NOT NULL,
	create_user VARCHAR(30) NOT NULL,
	modified_dt DATE NOT NULL,
	modified_app VARCHAR(30) NOT NULL,
	modified_user VARCHAR(30) NOT NULL,
	status BOOLEAN NOT NULL,
	note VARCHAR(100),
	FOREIGN KEY (cons_id) REFERENCES cons (cons_id),
	PRIMARY KEY (cons_email_id)
);`

`-- Create cons_email_chapter_subscription table
CREATE TABLE cons_email_chapter_subscription (
	cons_email_chapter_subscription_id INT NOT NULL,
	cons_email_id VARCHAR(10) NOT NULL,
	chapter_id INT NOT NULL,
	isunsub BOOLEAN NOT NULL,
	unsub_dt TIMESTAMP NOT NULL,
	modified_dt TIMESTAMP NOT NULL,
FOREIGN KEY (cons_email_id) REFERENCES cons_email (cons_email_id),
PRIMARY KEY (cons_email_chapter_subscription_id)	
);`

#### Inner Joins

`-- Create pre_people table
CREATE TABLE pre_people AS
SELECT cons.cons_id, 
cons.subsource, 
cons_email.cons_email_id, email, 
cons_email.is_primary, 
cons.create_dt AS created_dt, 
cons.modified_dt AS updated_dt, 
cons_email_chapter_subscription.isunsub  
FROM cons
INNER JOIN cons_email
ON cons.cons_id = cons_email.cons_id
INNER JOIN cons_email_chapter_subscription
ON cons_email.cons_email_id = cons_email_chapter_subscription.cons_email_id AND chapter_id = 1
WHERE cons_email.is_primary = true
;`

#### Created people table

`-- Create people table
CREATE TABLE people AS
SELECT email, 
subsource AS code, 
isunsub AS is_unsub, 
created_dt, 
updated_dt FROM pre_people;`

#### Exported people table

[link]
