# Data-Integration-Pipelines-for-NYC-Payroll-Data-Analytics
# Create high-quality data pipelines that are dynamic, can be automated, and monitored for efficient operation.


### Project Introduction
The City of New York would like to develop a Data Analytics platform on Azure Synapse Analytics to accomplish two primary objectives:
  - Analyze how the City's financial resources are allocated and how much of the City's budget is being devoted to overtime.
  - Make the data available to the interested public to show how the City’s budget is being spent on salary and overtime pay for all municipal employees.
  
As a Data Engineer, I have to create high-quality data pipelines that are dynamic, can be automated, and monitored for efficient operation. The project team also includes the city’s quality assurance experts who will test the pipelines to find any errors and improve overall data quality.

The source data resides in Azure Data Lake and needs to be processed in a NYC data warehouse in Azure Synapse Analytics. The source datasets consist of CSV files with Employee master data and monthly payroll data entered by various City agencies.

![image](https://user-images.githubusercontent.com/61830624/198897732-c3f75ee7-9e99-4faf-b096-2a9134cdb7ff.png)


### For this project, we will do our work in the Azure Portal, using several Azure resources including:
  - Azure Data Lake Gen2
  - Azure SQL DB
  - Azure Data Factory
  - Azure Synapse Analytics

# Step 1: Prepare the Data Infrastructure
Setup Data and Resources in Azure

### 1.Create the data lake and upload data

Create an Azure Data Lake Storage Gen2 (storage account) with three directories in this storage container named
  - dirpayrollfiles
  - dirhistoryfiles
  - dirstaging
  ![image](https://user-images.githubusercontent.com/61830624/198900600-f99cefe8-197c-4586-a946-d91fb4992b65.png)

Upload data from the project data to the dirpayrollfiles folder
![image](https://user-images.githubusercontent.com/61830624/198900724-0b64e006-dac5-401a-8db2-f511d115cc6f.png)


Upload `nycpayroll_2020.csv` file (historical data) from the project data to the dirhistoryfiles folder
![image](https://user-images.githubusercontent.com/61830624/198900847-5154755a-6cc0-44c4-a415-1b39562b91c9.png)

### 2. Create an Azure Data Factory Resource
![image](https://user-images.githubusercontent.com/61830624/198901149-9e7c1014-9af7-4e0b-ab97-bdc7f8fce9a8.png)

### 3. Create a SQL Database to store the current year of the payroll data
![image](https://user-images.githubusercontent.com/61830624/198901872-a8c8f05a-16f6-4e16-a457-c31a8c9d812a.png)

Create a table called `NYC_Payroll_Data` in db_nycpayroll in the Azure Query Editor with SQL Script:
![image](https://user-images.githubusercontent.com/61830624/198901976-af3c0a42-d03d-4964-8ffb-4ca8778b84e4.png)

### 4. Create A Synapse Analytics workspace
![image](https://user-images.githubusercontent.com/61830624/198902617-029a2be1-8dc0-45a3-a6f5-32a01be0d8e5.png)

Create a SQL dedicated pool in the Synapse Analytics workspace
  - Select DW100c as performance level. 
![image](https://user-images.githubusercontent.com/61830624/198903086-6c4b0725-6afa-4fec-b84c-70878b1d9505.png)

In the SQL dedicated pool, Create master data tables and payroll transaction tables:
![image](https://user-images.githubusercontent.com/61830624/198903195-98f7b2ba-cbef-4278-9243-8f81d3ca189c.png)

The table are successfully created in the SQL Database
![image](https://user-images.githubusercontent.com/61830624/198903272-07ca7114-ca11-4edd-8b4a-784c7feee37b.png)







