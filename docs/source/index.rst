ME AND YOU CAMPAIGN DOCUMENTATION
===================================

This Documentation Takes Users Through On How To Utilize Stored Procedures In SQL To Manage Leads From The Me And You Campaign, Apply Buisness Rules To
The Data And Insert The Data To The Appropriate Tables In Snowflake. 


How To Create A Stored Procedure In SQL
---------------------------------------

.. code-block::
   
     CREATE OR REPLACE PROCEDURE "SP_API_CAMPAIGN_NAME_BUSINESSRULES"
     ("LEADS" NUMBER(38,0))
     RETURNS VARCHAR(16777216)
     LANGUAGE SQL
     EXECUTE AS OWNER
     
     
     
     
     
     
     
     
     
     
     
 **In The First Line We Are Creating A Procedure Called SP_API_CAMPAIGN_NAME_BUSINESSRULES**
 
 **In The Second Line, The Procedure Takes A Parameter Stored In A Variable Called Leads And Leads Is A Number Datatype With A Maximum Of 38 digits.**
 **The Procedure Returns A Variable Length Character Datatype And The Procedure Is Utilized In the SQL Language By The Owner Of The Procedure**
 
 


