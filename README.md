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


# Step 2: Create Linked Services

### 1.Create a Linked Service for Azure Data Lake

In Azure Data Factory, create a linked service to the data lake that contains the data files
  - From the data stores, select Azure Data Lake Gen 2
  - Test the connection
![image](https://user-images.githubusercontent.com/61830624/198904731-9f6bd086-9ff9-4c8c-97e1-9930f17a85c5.png)

### 2.Create a Linked Service to SQL Database that has the current (2021) data
If you get a connection error, remember to add the IP address to the firewall settings in SQL DB in the Azure Portal
![image](https://user-images.githubusercontent.com/61830624/198904792-ec4b97d3-3b3e-48f5-affd-d7e680457e2f.png)


### 3. Create a Linked Service for Synapse Analytics
  - Create the linked service to the SQL pool.
![image](https://user-images.githubusercontent.com/61830624/198904879-03b6b449-3b73-42b5-a737-b4259946002b.png)

All the 3 linked services successfully created:
![image](https://user-images.githubusercontent.com/61830624/198904895-2d2cf65c-020b-4942-924a-36086f7f35c5.png)

# Step 3: Create Datasets in Azure Data Factory
### 1.Create the datasets for the 2021 Payroll file on Azure Data Lake Gen2
  - Select DelimitedText
  - Set the path to the nycpayroll_2021.csv in the Data Lake
  - Preview the data to make sure it is correctly parsed
![image](https://user-images.githubusercontent.com/61830624/198905426-52749c78-c4d3-4704-8669-47cec2408e7b.png)

![image](https://user-images.githubusercontent.com/61830624/198905392-da6ddb68-1deb-4d04-96b1-d0f0ca0b65c2.png)

### 2. Repeat the same process to create datasets for the rest of the data files in the Data Lake
  - EmpMaster.csv
  - TitleMaster.csv
  - AgencyMaster.csv
  - Remember to publish all the datasets
![image](https://user-images.githubusercontent.com/61830624/198905378-eefe9482-f5cb-484b-8d32-f388f8ae33a9.png)

### 3. Create the dataset for transaction data table that should contain current (2021) data in SQL DB
![image](https://user-images.githubusercontent.com/61830624/198905778-5ecca5d3-43e4-4684-a7b3-9eadbd0e3285.png)

### 4. Create the datasets for destination (target) tables in Synapse Analytics
  - dataset for NYC_Payroll_EMP_MD
   ![image](https://user-images.githubusercontent.com/61830624/198905948-940f781b-d82e-4cb7-af85-7d4d83acf1b5.png)

  - for NYC_Payroll_TITLE_MD
    ![image](https://user-images.githubusercontent.com/61830624/198906015-ddf13554-b934-447d-be7a-61c206590d62.png)

  - for NYC_Payroll_AGENCY_MD
    ![image](https://user-images.githubusercontent.com/61830624/198906059-81387635-34ba-498c-994e-1fb22a3140bb.png)

  - for NYC_Payroll_Data
    ![image](https://user-images.githubusercontent.com/61830624/198906088-fed9842e-867c-46c5-bf24-b2899a83fa1d.png)


# Step 4: Create Data Flows

### 1 .In Azure Data Factory, create the data flow to load 2021 Payroll Data to SQL DB transaction table (in the future NYC will load all the transaction data into this table).
  - Create a new data flow
  - Select the dataset for the 2021 payroll file as the source
    ![image](https://user-images.githubusercontent.com/61830624/198999545-ff9a20ba-0b23-422f-8188-7152c02c516f.png)
    
    ![image](https://user-images.githubusercontent.com/61830624/198998800-6037d9ef-ee12-483c-8975-4fc5bb101334.png)
  
  - Select the sink dataset as the payroll table on SQL DB
    ![image](https://user-images.githubusercontent.com/61830624/198999423-a81a99ea-b09e-45a6-bae7-242ee2f6abf3.png)

  - Make sure to reassign any missing source to target mappings
    ![image](https://user-images.githubusercontent.com/61830624/198998870-4849c7a9-a4c3-48a3-bdfd-2aa73155a018.png)

### 2.Create Pipeline to load 2021 Payroll data into transaction table in the SQL DB
  - Create a new pipeline
  - Select the data flow to load the 2021 file into SQLDB
    ![image](https://user-images.githubusercontent.com/61830624/199007103-a695671e-9789-4d58-8a94-64123c9a9fb6.png)

  - Trigger the pipeline
     ![image](https://user-images.githubusercontent.com/61830624/199007394-42da9439-b454-4a71-93cd-868bff46157d.png)

  - Monitor the pipeline
     ![image](https://user-images.githubusercontent.com/61830624/199008169-376a7057-e07b-435c-9ce3-72ab64104e52.png)

  - Take a screenshot of the Azure Data Factory screen pipeline run after it has finished.
    ![image](https://user-images.githubusercontent.com/61830624/199008048-58a6aee3-e716-4a38-a554-2abe955cb697.png)

  - Make sure the data is successfully loaded into the SQL DB table

