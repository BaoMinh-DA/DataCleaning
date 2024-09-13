[club_member_info_CLEANED_202409131825.md](https://github.com/user-attachments/files/16993100/club_member_info_CLEANED_202409131825.md)[club_member_info_202409131812.md](https://github.com/user-attachments/files/16992956/club_member_info_202409131812.md)# DataCleaning
introduction
This is an educational project on data cleaning and preparation using SQL. The original database in CSV format is located in the file club_member_info.csv. Here, we will explore the steps that need to be applied to obtain a cleansed version of the dataset.

IMPORT club_member_info.csv in to DBeaver

SELECT *
FROM club_member_info
LIMIT 10;

[Uploading club_member_info_2024|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|addie lush|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|      ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|Sydel Sharvell|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|Constantin de la cruz|35||co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|  Gaylor Redhole|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|
|Wanda del mar       |44|single|wkunzel5@slideshare.net|937-467-6942|10864 Buhler Plaza,Hamilton,Ohio|Human Resources Assistant IV|3/24/2015|
|Joann Kenealy|41|married|jkenealy6@bloomberg.com|513-726-9885|733 Hagan Parkway,Cincinnati,Ohio|Accountant IV|4/17/2013|
|   Joete Cudiff|51|divorced|jcudiff7@ycombinator.com|616-617-0965|975 Dwight Plaza,Grand Rapids,Michigan|Research Nurse|11/16/2014|
|mendie alexandrescu|46|single|malexandrescu8@state.gov|504-918-4753|34 Delladonna Terrace,New Orleans,Louisiana|Systems Administrator III|3/12/1921|
| fey kloss|52|married|fkloss9@godaddy.com|808-177-0318|8976 Jackson Park,Honolulu,Hawaii|Chemical Engineer|11/5/2014|
09131812.md…]()

###Step 1: Create a new table for cleaning
Let's generate a new table where we can manipulate and restructure the data without modifying the original dataset.
CREATE TABLE club_member_info_cleaned (
	full_name VARCHAR(50),
	age INTEGER,
	marital_status VARCHAR(50),
	email VARCHAR(50),
	phone VARCHAR(50),
	full_address VARCHAR(50),
	job_title VARCHAR(50),
	membership_date VARCHAR(50)
);

###Steps 2: Copy all values from original table
INSERT INTO club_member_info_cleaned
SELECT * FROM club_member_info;

###Steps 3: Trim whitespaces
/*UPDATE club_member_info_CLEANED 
SET full_name = TRIM(full_name)*/

###Steps 3: To uppercase
/*UPDATE club_member_info_CLEANED 
SET full_name = UPPER(full_name)*/

###Steps 4: Replace empty values with NULL
UPDATE club_member_info_cleaned SET 
	full_name = CASE WHEN full_name = '' THEN NULL ELSE full_name END;
 
###Steps 5:
SELECT COUNT(*) FROM club_member_info_cleaned 
	WHERE age < 18 OR age > 90 or age is NULL;
 UPDATE club_member_info_cleaned 
	SET age = (SELECT MODE(age) FROM club_member_info_cleaned) 
	WHERE age < 18 OR age > 90 or age is NULL;

###Steps 6: Marital status cleaning
UPDATE club_member_info_cleaned 
	SET marital_status = TRIM(LOWER(marital_status));
 UPDATE club_member_info_cleaned 
	SET marital_status = NULL 
	WHERE NOT marital_status IN ('single', 'married', 'divorced');

###Steps 7: Other text columns
UPDATE club_member_info_cleaned SET
	phone = TRIM(LOWER(phone)),
	full_address = TRIM(LOWER(full_address)),
	job_title = TRIM(LOWER(job_title)),
	membership_date  = TRIM(LOWER(membership_date));

 UPDATE club_member_info_cleaned SET
	phone = NULL WHERE phone = '';
UPDATE club_member_info_cleaned SET
	full_address = NULL WHERE full_address = '';
UPDATE club_member_info_cleaned SET
	job_title = NULL WHERE job_title = '';
UPDATE club_member_info_cleaned SET
	membership_date = NULL WHERE membership_date = '';

 SELECT * FROM club_member_info_cleaned LIMIT 10;
[Uploading club_member_info_CLEANED_202409|full_name|age|marital_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|ADDIE LUSH|40|married|alush0@shutterfly.com|254-389-8708|3226 eastlawn pass,temple,texas|assistant professor|7/31/2013|
|ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 harbort avenue,fayetteville,north carolina|programmer iii|5/27/2018|
|SYDEL SHARVELL|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 school place,las vegas,nevada|budget/accounting analyst i|10/6/2017|
|CONSTANTIN DE LA CRUZ|35||co3@bloglines.com|402-688-7162|6 monument crossing,omaha,nebraska|desktop support technician|10/20/2015|
|GAYLOR REDHOLE|38|married|gredhole4@japanpost.jp|917-394-6001|88 cherokee pass,new york city,new york|legal assistant|5/29/2019|
|WANDA DEL MAR|44|single|wkunzel5@slideshare.net|937-467-6942|10864 buhler plaza,hamilton,ohio|human resources assistant iv|3/24/2015|
|JOANN KENEALY|41|married|jkenealy6@bloomberg.com|513-726-9885|733 hagan parkway,cincinnati,ohio|accountant iv|4/17/2013|
|JOETE CUDIFF|51|divorced|jcudiff7@ycombinator.com|616-617-0965|975 dwight plaza,grand rapids,michigan|research nurse|11/16/2014|
|MENDIE ALEXANDRESCU|46|single|malexandrescu8@state.gov|504-918-4753|34 delladonna terrace,new orleans,louisiana|systems administrator iii|3/12/1921|
|FEY KLOSS|52|married|fkloss9@godaddy.com|808-177-0318|8976 jackson park,honolulu,hawaii|chemical engineer|11/5/2014|
131825.md…]()


