# Automating a Scalable Data Pipeline with AWS Glue, Athena, and Terraform

## Project Overview

This repository contains the code and configurations to build a fully automated and scalable data pipeline. The pipeline extracts data from a [MySQL SDatabase](https://www.mysqltutorial.org/mysql-sample-database.aspx) on AWS RDS, transforms it into a star schema using AWS Glue, and loads the transformed data into Amazon S3 for querying with Amazon Athena. Terraform is used to manage infrastructure as code, ensuring repeatability and scalability.

## Table of Contents
- [Architecture](#architecture)
- [Technologies Used](#technologies-used)
- [Project Setup](#project-setup)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Running the Pipeline](#running-the-pipeline)
- [Challenges and Solutions](#challenges-and-solutions)
- [Conclusion](#conclusion)


## Architecture

![Data Pipeline Architecture](/Users/freDelicious/Documents/git/DataPipeline Project/images/ETL.drawio.png)

The data pipeline includes the following stages:
1. **Data Ingestion**: Extract data from MySQL on AWS RDS.
2. **Transformation**: Use AWS Glue to clean and transform the data into a star schema.
3. **Data Storage**: Load the transformed data into Amazon S3.
4. **Data Querying**: Query the data using Amazon Athena.
5. **Visualization**: Create interactive dashboards with Jupyter Lab for data exploration.

## Technologies Used

- [AWS Glue](https://aws.amazon.com/en/glue/): For ETL (Extract, Transform, Load) processing.
- **Amazon RDS (MySQL)**: Source database for storing transactional data.
- **Amazon S3**: Scalable object storage for both raw and transformed data.
- [Amazon Athena](https://aws.amazon.com/en/athena/): Serverless query service to analyze data stored in S3.
- **Jupyter Lab**: For data visualization and analysis.
- [Terraform](https://www.terraform.io/): Infrastructure as Code (IaC) tool to automate AWS resource provisioning.

## Project Setup

### Prerequisites
- AWS account
- Python 3.9+
- Terraform installed (v1.0+)
- AWS CLI configured with appropriate permissions

### Installation

1. **Clone the repository**:
    ```bash
    git clone https://github.com/your-repo/data-pipeline-aws.git
    cd data-pipeline-aws
    ```

2. **Initialize Terraform**:
    ```bash
    terraform init
    ```

3. **Configure Terraform**:
    Adjust the `variables.tf` file to set your AWS region, RDS instance details, and S3 bucket names.
    The `tf` files you checked allow you to declare the resources of your data 
    architecture. You still need to specify for AWS Glue how to perform data 
    extraction, transformation, and load. You can check all of these steps as Python 
    script [glue_job.py](jupyterlab/glue_job.py). 

4. **Deploy the infrastructure**:
    ```bash
    terraform apply
    ```

5. **Configure and start AWS Glue jobs**:
    Follow the instructions in `glue_job.py` to configure your Glue job. You can start the job using the AWS CLI:
    ```bash
    aws glue start-job-run --job-name <your-glue-job-name>
    ```

## Running the Pipeline

Once the infrastructure is deployed and Glue jobs are running:

1. **Ingest Data**: AWS Glue extracts data from MySQL.
2. **Transform Data**: The data is transformed into a star schema and stored in Amazon S3.
3. **Query with Athena**: Use Athena to run SQL queries on the transformed data. Example query:
    ```sql
    SELECT country, SUM(orderAmount) AS total_sales
    FROM fact_orders
    GROUP BY country
    ORDER BY total_sales DESC;
    ```

4. **Data Visualization**: Use Jupyter Lab notebooks to visualize the data. Example code to visualize total sales per country:
    ```python
    import pandas as pd
    import seaborn as sns
    df = pd.read_csv('s3://path-to-your-data/sales_data.csv')
    sns.barplot(x='Country', y='Total Sales', data=df)
    ```

## Challenges and Solutions

- **Optimizing Glue Job Performance**: Initially, Glue jobs were slow due to resource limitations. Increasing the number of workers and using pushdown predicates optimized performance.
- **Efficient Querying**: Partitioning data in S3 by date and country improved Athena’s query performance and reduced costs.
- **Schema Management**: AWS Glue’s DynamicFrames were used to handle schema evolution and mismatches between MySQL and the transformed data.


## Conclusion
Feel free to customize this README by replacing placeholders (like the image path and repository URL) and adding any additional instructions or links.

Let us know if you encounter any issues or have ideas for improvements! 🚀

**Author:** ✍️

- Freda Victor
- [Project Blog](https://learndataengineering.hashnode.dev/building-an-automated-job-scraping-system-on-aws-challenges-solutions-and-insights).
- Date: 17 September 2024
