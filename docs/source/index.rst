ME AND YOU CAMPAIGN DOCUMENTATION
===================================

This Documentation Takes Users Through On How To Utilize Stored Procedures In SQL To Manage Leads From The Me And You Campaign, Apply Buisness Rules To
The Data And Insert The Data To The Appropriate Tables In Snowflake. 


How To Create A Stored Procedure In SQL
---------------------------------------

     ``CREATE OR REPLACE PROCEDURE "SP_API_CAMPAIGN_NAME_BUSINESSRULES"``
     
     ``("LEADS" NUMBER(38,0))``
     
     ``RETURNS VARCHAR(16777216)``
     
     ``LANGUAGE SQL``
     
     ``EXECUTE AS OWNER``
   
  
   
 In The First Line We Are Creating A Procedure Called SP_API_CAMPAIGN_NAME_BUSINESSRULES.
 In The Second Line, The Procedure Takes A Parameter Stored In A Variable Called Leads And Leads Is A Number Datatype With A Maximum Of 38 digits.
 The Procedure Returns A Variable Length Character Datatype And The Procedure Is Utilized In the SQL Language By The Owner Of The Procedure.
 
 
 
.. note:: 
 
   **AS ‘**

  **BEGIN**

  *In SQL Procedures, This Is The Body Of The Procedure And Everything Inside Here Will Be Executed.*

  **End;**
  
  
  
  
  
Calling A Procedure In SQL
--------------------------

.. code-block::

    CALL 
    DATAWAREHOUSE.DISTRIBUTION_DATA_APPLICATION.SP_API_ME_AND_YOU_UNLIMITED_HISTORY_INSERT(:LEADS);
    
    
In The Above Code, We Use The 'CALL' Keyword To Summon And Run The Procedure Called **SP_API_ME_AND_YOU_UNLIMITED_HISTORY_INSERT**.
This procedure is stored in a database called DATAWAREHOUSE and the schema DISTRIBUTION_DATA_APPLICATION.This procedure takes the parameter “Leads” which is a number with a maximum of 38 digits and inserts into the history table.Remember, The Parameter Is passed As A Number And Returned As A VARCHAR.



``CALL DATAWAREHOUSE.DISTRIBUTION_DATA_APPLICATION.SP_AUTORANK(11225,6);``


*The above line of code takes two parameters. The first parameter specifies which campaign you referring to and this known as a campaign ID. The second parameter specifies how many days you want to rank the data. In the above example, you are ranking for 6 days. The leads will distributed evenly throughout all the days in each scoregroup.*




DECLARING VARIABLES WITHIN PROCEDURES IN SQL
--------------------------------------------

.. code-block::

   declare
   max integer;
   scoregroup varchar;
   leadsload integer;
   
   
In The Block Of Code Above, We Use The **DECLARE**  keyword To Declare:

*a) A Variable Called Max That Stores An Integer Data Type*

*b) A Variable Called scoregroup That Stores A Varchar Data Type*

*c) A Variable Called Leadsload That Stores An Integer Data Type*



Creating A Table From A View In SQL 
-----------------------------------

**Note:**

A view is a virtual table containing rows and columns from a real table. 



.. code-block::

   CREATE OR REPLACE TABLE DATAWAREHOUSE.DISTRIBUTION_DATA_APPLICATION.TS_API_CAMPAIGN_NAME_STAGE

   AS SELECT *

   FROM DATAWAREHOUSE.DISTRIBUTION_DATA_APPLICATION.VW_IK_ME_AND_YOU_UNLIMITED_AUTOMATION_DATAPOOL;
   
   
*In this code above, we are creating a table called TS_API_CAMPAIGN_NAME_STAGE in the database called datawarehouse and IN the DISTRIBUTION_DATA_APPLICATION schema. Essentially we are creating the table TS_API_CAMPAIGN_NAME_STAGE from a view called VW_IK_CAMPAIGN_NAME_AUTOMATION_DATAPOOL.This view contains all leads (data) hence the name “datapool”. The newly created table will be populated with all the columns and rows from the view.*



 
 


