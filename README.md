# Bing News Data Analytics Project

## Project Overview
This project involves the creation of a Bing News Data Analytics platform using Microsoft Fabric. The main objective is to ingest news data from Bing into Microsoft Fabric, process this data, and then visualize it through a PowerBI dashboard.

## What is Bing?
Bing is a search engine provided by Microsoft, similar to Google. It is integrated into the Microsoft Edge browser as the default search engine. Bing offers various APIs, including a News Search API, which allows developers to retrieve the latest news articles and information programmatically.

## Project Steps

### Project Setup

#### Step 1: Using Bing API
Create and configure the Bing News Search API as a data source.

#### Step 2: Data Ingestion
Use Data Factory to copy all the data from the Bing News API into the Fabric workspace.

### Data Processing and Visualization

#### Step 3: Data Processing in Fabric
Process the ingested news data within the Fabric workspace to clean and prepare it for analysis.

#### Step 4: Creating a PowerBI Dashboard
Utilize PowerBI to create a comprehensive news dashboard. This dashboard will display all the latest news of the day and provide various analytics on the news data.

![image2](https://github.com/ShrutiParulekar/Bing_news_data_analytics/assets/30751858/70b369a4-4bf1-411e-9f4e-f3583304fa75)


## Additional Setup Steps

### Setup Resource Group in Azure Portal
1. **Create a Resource Group**
    - Created a separate Resource Group for efficient resource management.

### Configure Bing News Search API
2. **Set Up Bing News Search API**
    - Set up the Bing News Search API within the Resource Group.

### Establish Power BI Workspace
3. **Create a Power BI Workspace**
    - Created a dedicated workspace in Power BI.
    - Enabled Microsoft Fabric in the workspace.

### Configure Synapse Data Engineering
4. **Create a Lakehouse Database**
    - Created a Lakehouse database for data storage.


![image4](https://github.com/ShrutiParulekar/Bing_news_data_analytics/assets/30751858/400f4cde-02f6-42e8-be57-b63bc78ec8c6)

![image1](https://github.com/ShrutiParulekar/Bing_news_data_analytics/assets/30751858/a0da1a88-9cfc-4433-ace3-c49784cfcd5d)

## Data Transformation

In this data transformation project, I utilized Synapse Data Engineering to process raw JSON data from the Bing News API stored in a Lakehouse database. I created a Spark notebook to read the JSON file and transformed its structure by selecting the value column and using the explode function to convert JSON objects into multiple rows. By converting these JSON strings to Python dictionaries, I extracted key information such as title, description, category, URL, image, provider, and published date. This data was then combined into a structured DataFrame with properly formatted dates. Finally, I wrote the cleaned and structured data to the Lakehouse database as a Delta table, making it ready for further analysis and reporting.

### Why Incremental Load?

To ensure efficient data updates, I implemented incremental loading instead of overwriting the entire dataset. Incremental loading is preferred as it allows for the addition of new data without reprocessing the entire dataset, saving time and computational resources. This method reduces the risk of data loss and maintains historical data integrity, ensuring that only new or updated records are added to the Delta table. This approach is crucial for handling large datasets where frequent updates are necessary, providing a more efficient and scalable solution.

- **[Link of the Python Notebook](https://github.com/ShrutiParulekar/Bing_news_data_analytics/blob/main/Power%20BI%20dashboard/News-dashboard.pdf)** 
- **Link of the Dataset Created:** Data transformation/main_data.csv

## Sentiment Analysis using Synapse ML

In this section of the project, I performed sentiment analysis using Synapse Data Science within Microsoft Fabric. I began by accessing the Synapse Data Science workspace and creating a new notebook. I attached the existing Lakehouse database (Bing_Lake_DB) to the notebook to access the clean Delta table. Using Synapse ML, I imported the pre-trained TextBlob model and configured it to analyze the description column of the news articles for sentiment. After applying the model, I extracted the sentiment values and cleaned the DataFrame by removing unnecessary columns. Finally, I used incremental loading with Python Merge logic to write the final DataFrame back to the Lakehouse database as a Delta table named `TBL_Sentiment_Analysis`, ensuring efficient data updates and maintaining historical data integrity.

- **Link of the Python Notebook:** Sentiment analysis
- **Link of the Dataset Created:** Sentiment analysis/sentiment analysis.csv

## PowerBI Dashboard for Sentiment Analysis of Latest News

I created a PowerBI dashboard connected to the Lakehouse database using a semantic model. This dashboard displays the latest news and includes sentiment analysis, showing the percentage of positive, negative, and neutral sentiments based on the news descriptions stored in the database.

- **PowerBI file:** Power BI dashboard/News-dashboard.pbix
- **PDF File:** Power BI dashboard/News-dashboard.pdf

<img width="1227" alt="Screen Shot 2024-06-23 at 4 19 21 PM" src="https://github.com/ShrutiParulekar/Bing_news_data_analytics/assets/30751858/2c640a25-3203-48e7-9ef4-ec32626fe173">

## Creating a Data Pipeline

### Building Pipelines using Data Factory

To orchestrate the tasks completed in our end-to-end Azure Data Engineering project, I created a pipeline using Data Factory. The process involved several key steps:

#### Step 1: Data Injection
- Created a Data Factory pipeline named `news_inje_pipeline`.
- Configured the pipeline to connect to the Bing API and copy news articles in JSON format into the Lakehouse database.

#### Step 2: Data Transformation
- Added a notebook activity to transform the raw JSON file into a clean and structured Delta table.
- Configured the notebook activity to run after the data injection step.

#### Step 3: Sentiment Analysis
- Added another notebook activity to perform sentiment analysis on the news articles.
- Configured this notebook to run after the data transformation step.

#### Parameterizing the Search Term
- Created a parameter for the search term to make the pipeline more flexible.
- Updated the relative URL in the data injection step to use this parameter, allowing for dynamic search terms.

#### Scheduling the Pipeline
- Scheduled the pipeline to run daily at 6:00 AM to ingest the latest news articles and update the PowerBI report with the new data and sentiment analysis.

![image3](https://github.com/ShrutiParulekar/Bing_news_data_analytics/assets/30751858/dc6b8059-86e2-419f-8af6-2c28fca417a7)

### Setting up Alerts

In the final part of the project, I set up alerts in PowerBI reports using Data Activator. I navigated to the Data Activator component within the workspace and created a new reflex item to manage and monitor alerts. I then moved to the PowerBI dashboard, specifically targeting the positive sentiment card visual for alert configuration. After selecting the visual, I set up an alert condition to trigger when the positive sentiment percentage exceeds 0%, choosing to receive notifications via Microsoft Teams. I tested the configuration with a test alert to ensure proper functionality.

### End-to-End Pipeline Testing

Finally, I performed an end-to-end pipeline test by running the news injection pipeline with a new search term. This pipeline successfully ingested the latest news articles and updated the PowerBI report. The positive sentiment percentage was updated, and I received the configured Teams alert, confirming the system's effectiveness.

This comprehensive setup ensures that the PowerBI report is automatically updated with the latest data and sentiment analysis, with alerts notifying us of significant changes.

