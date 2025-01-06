# FETCH_work
Coding exercise 
 ~ SQL dialect = T-SQL

--------------------------------------------------------------------------------------------------------
# (1) What questions do you have about the data?
 q: - why is there missing information in the "users.json" file? Specifically, what is significance of a blank "lastloginDate"?
    - was the field "signupsource" in the 'users.json' file intentionally left out of the schema ? 
    - is there a designated foreign key to join the "brand_data" with "receipts_data" & "users_data" ?
    - why is there missing information in the "brands.json" file? Specifically, what is definition for category and how is that information captured ? 
    - why are there inconsistencies within records for "category" and "categoryCode" values ? 
    
--------------------------------------------------------------------------------------------------------
# (2) How did you discover the data quality issues?
 answer: querying the users_data table, I input paramaters to search for a missing value in either the createDate, lastlogin, or state fields so that we can see how many records are incomplete/missing this data.
Since date data and geogpraphic location data should be present in every valid record I chose these fields to test, but all fields should be visible in the resulting table (temp_blanks).  
 [
   create table temp_blanks as 
      select *
       from users_data
       where createdDate IS NULL OR lastlogin IS NULL OR state IS NULL
     order by _id, createdDate, lastlogin, state desc ;
  ]
.
.
.
# NOTE: 
since the other fields in this table are static or maintained by the company they were not included in this query for missing values. Although there are missing values in the "signupsource" (a field that was not listed in the schema, but was imported in the intial load), they can be designated as "Others" or "Not Specified" in order to improve data quality. 

--------------------------------------------------------------------------------------------------------  
# (3) What do you need to know to resolve the data quality issues?
 answer: - what is the cause of missing datetime information ? 
         - why are there inconsistenciees between the "category" and "categoryCode" fields in the brands.json data file ? (ex. record #1: category='Beverages', categoryCode='BEVERAGES' | record #2: category='Beverages', categoryCode=(blank)) 
        - what is the process for mapping from one value to the other ? 

--------------------------------------------------------------------------------------------------------  
# (4) What other information would you need to help you optimize the data assets you're trying to create?
  answer: - a clearly defined foreign key to join the brand.json file data with the proposed 'fact_data' data table. 
          - what is cleaning or preparation (if any) has been done to the files prior to me receiving them ?

--------------------------------------------------------------------------------------------------------  
(5) What performance and scaling concerns do you anticipate in production and how do you plan to address them?
  answer: - linking "brand_data" to the "fact_data" table without a designated foreign key. it will not be possible to accurately integrate that information into the fact_data table without some linkage, similar to the "users_data" & "receipts_data" join keys. 
          - using text values in category, categoryCode, brandCode fields could lead to error. I suggest using ods codes. 
  
