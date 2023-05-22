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
    DATAWAREHOUSE.DISTRIBUTION_SCHEMA.SP_API_CAMPAIGN_NAME_HISTORY_INSERT(:LEADS);
    
    
In The Above Code, We Use The 'CALL' Keyword To Summon And Run The Procedure Called **SP_API_CAMPAIGN_NAME_HISTORY_INSERT**.
This procedure is stored in a database called DATAWAREHOUSE and the schema DISTRIBUTION_SCHEMA.This procedure takes the parameter “Leads” which is a number with a maximum of 38 digits and inserts into the history table.Remember, The Parameter Is passed As A Number And Returned As A VARCHAR.



``CALL DATAWAREHOUSE.DISTRIBUTION_SCHEMA.RANKING_PROCEDURE(11225,6);``


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

   CREATE OR REPLACE TABLE DATAWAREHOUSE.DISTRIBUTION_SCHEMA.TS_API_CAMPAIGN_NAME_STAGE

   AS SELECT *

   FROM DATAWAREHOUSE.DISTRIBUTION_SCHEMA.VW_IK_CAMPAIGN_NAME_AUTOMATION_DATAPOOL;
   
   
*In this code above, we are creating a table called TS_API_CAMPAIGN_NAME_STAGE in the database called datawarehouse and in the DISTRIBUTION_DATA_APPLICATION schema. Essentially we are creating the table TS_API_CAMPAIGN_NAME_STAGE from a view called VW_IK_CAMPAIGN_NAME_AUTOMATION_DATAPOOL.This view contains all leads (data) hence the name “datapool”. The newly created table will be populated with all the columns and rows from the view.*



CREATING CTE'S IN SQL
=====================


WHAT IS A CTE ?
---------------

CTE, also known as a common table expression works as virtual tables that are created during execution of a query, used by that query and removed or deleted 
after the query has finished execution.


.. code-block::


    CREATE OR REPLACE TABLE DATAWAREHOUSE.DISTRIBUTION_DATA_APPLICATION.TS_API_CAMPAIGN_NAME_STAGE_BANDS
    AS
    WITH MAIN AS (
    select
    COUNT(1) as LEADS,
    SCOREGROUP
    from DATAWAREHOUSE.DISTRIBUTION_DATA_APPLICATION.TS_API_CAMPAIGN_NAME_UNLIMITED_STAGE A
    group by SCOREGROUP order by SCOREGROUP
    ),
    TOTAL AS (
    SELECT SUM(LEADS) AS TOTALLEADS
    FROM MAIN
    )
    
    
*In the code above we are creating a table called TS_API_ME_CAMPAIGN_NAME_STAGE_BANDS.*

*The “with main” as line of code creates a temporary table (CTE) and stores the column called LEADS as “count” which is a column containing the total number of leads and scoregroup from the stage table. It will also output the scoregroup from the TS_API_CAMPAIGN_NAME_STAGEBANDS table and order the scoregroup column in ascending order from lowest scoregroup to highest scoregroup. The query called TOTAL will output the total number of leads and store it as totalleads from the temporary table called main.*




Working With The Row Number Function And Partition Keyword In SQL
-----------------------------------------------------------------

.. code-blocks::

    SELECT
    ROW_NUMBER () OVER (PARTITION BY ''SCOREGROUP'' ORDER BY SCOREGROUP) AS IDENTITY,
    SCOREGROUP,
    LEADS,
    round(LEADS/(SELECT TOTALLEADS FROM TOTAL)*100,2) AS PERCENTAGE,
    round(LEADS/(SELECT TOTALLEADS FROM TOTAL)*(:LEADS),0) AS LEADSTOLOAD
    FROM MAIN;



*The row_number function is used to assign each row a sequential integer. The partition by keyword will divide the rows by scoregroups. Essentially different      scoregroups will be divided and sorted in ascending order from lowest scoregroup to highest scoregroup sequentially. The total number of rows will also be        returned as LEADS. A percentage column will also be created which will contain the total number of records returned as a percentage rounded to 2 decimal          places.*
   
   
   
   
   
Storing The Biggest Row Number Into A Variable Called Max
---------------------------------------------------------


We Can Do This By The Following Block Of Code : 


.. code-block::
    
    
    SELECT
    max(IDENTITY) into :max
    FROM DATAWAREHOUSE.DISTRIBUTION_DATA_APPLICATION.TS_API_CAMPAIGN_NAME_STAGE_BANDS;
    LET counter := 1;
    WHILE (counter <= max) DO
    select
    SCOREGROUP,
    LEADSTOLOAD into :scoregroup,leadsload
    from DATAWAREHOUSE.DISTRIBUTION_DATA_APPLICATION.TS_API_CAMPAIGN_NAME_STAGE_BANDS
    where IDENTITY = :counter;
    SELECT
    max(IDENTITY) into :max
    FROM DATAWAREHOUSE.DISTRIBUTION_DATA_APPLICATION.TS_API_CAMPAIGN_NAME_STAGE_BANDS;
    
    
*In this block of code above we are taking the last row (biggest row number) and storing it into a variable max which we declared earlier*


Working With Counters In SQL
---------------------------

.. code-block::

   LET counter := 1;
   WHILE (counter <= max) DO
   select
   SCOREGROUP,
   LEADSTOLOAD into :scoregroup,leadsload
   from DATAWAREHOUSE.DISTRIBUTION_DATA_APPLICATION.TS_API_CAMPAIGN_NAME_STAGE_BANDS
   where IDENTITY = :counter;
   
   
   
*This block of code is used to set the counter to 1 (meaning we start counting from 1) and while the row is not the last row, the scoregroup and leadsload can be inserted into SCOREGROUP and LEADSTOLOAD respectively.*



Inserting Fields Into A Table SQL
---------------------------------

.. code-block::

   insert into DATAWAREHOUSE.DISTRIBUTION_DATA_APPLICATION.HISTORY_NAME_LEADSLOADED (
   IDNUMBER,
   CAMPAIGNID,
   BATCHNAME,
   CREATEDONDATE,
   LEADEXPIRY,
   DATATYPE,
   CUSTOMERNAME,
   LASTNAME,
   CELLNUMBER,
   CONTACTNUMBER1,
   SCORE,
   SCOREGROUP)
   
   
   
*This Line of code above inserts these fields into the table called HISTORY_NAME_LEADSLOADED*


   
Mapping Values In SQL
---------------------

In the code below, the columns that are inserted into the table TM_HLL_HISTORYLEADSLOADED are mapped with the values defined and corresponding with the select statement. For example, the a.idnumber values from TS_API_CAMPAIGN_NAME_STAGE A table will be placed inside IDNUMBER column in the TM_HLL_HISTORYLEADSLOADED table.

.. code-block::

    Select
    a.idnumber,
    a.campaignid,
    a.batchname,
    a.createdondate,
    a.leadexpiry,
    a.datatype,
    a.customername,
    a.lastname,
    a.phone_number1,
    a.contactnumber1,
    a.score,
    a.scoregroup
    from DATAWAREHOUSE.DISTRIBUTION_DATA_APPLICATION.TS_API_CAMPAIGN_NAME_STAGE A
    where A.SCOREGROUP =: scoregroup
    and A.SCOREGROUPROWNUM <=: leadsload;
    counter := counter + 1;
    end while;
    return counter-1;
    end;
    
    
    
This block of code below is used to iterate or loop through the different records to be added. When all the records have been inserted, the while loop which is used to loop through all records is terminated.


``counter := counter + 1;``

``end while;``

``return counter-1;``



 
 


