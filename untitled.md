# Projects



## Building ETL pipeline to load public data to Hive (March 2021)

Goal

To provide the market price of fresh produce to business team.

Task

Daily data sync into our database

Action

1. Extract market price data in JSON format using public API that Korean government provides
2. Transform raw data to a table in a tabular format
3. Unload the data to S3
4. Create Redshift and Hive tables using the unloaded file in S3
5. Automate #1 to #4 using Airflow

Result

Provide reliable data to the business team. Before this process set in place, the business team manually had pulled and processed the data in Excel. The estimated time saving for the team is 40 minutes per day.&#x20;

##
