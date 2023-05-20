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


