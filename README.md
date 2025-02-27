# Data Project: Card Brand Performance Tracker <br>

<br>
<br>
<br>

![cardbranddashboardgif](https://github.com/user-attachments/assets/4af1cd10-622b-4bc4-b58f-e424e5050df5)


<br>
<br>
<br>

# Table of contents

- [Objective](#objective)
- [Data Source](#data-source)
- [Stages](#stages)
- [Design](#design)
  - [Tools](#tools)
  - [Dashboard Mockup](#dashboard-mockup)
- [Development](#development)
  - [Data Exploration Notes](#data-exploration-notes)
  - [Data Cleaning](#data-cleaning)
  - [Transform the Data and Creating the SQL View](#transform-the-data-and-creating-the-sql-view)
  - [Data Type Check](#data-type-check)
- [Visualization](#visualization)
  - [What does the dashboard look like?](#what-does-the-dashboard-look-like) 
  - [DAX Measures](#dax-measures)
- [Analysis](#analysis)
   - [Discovery, what did I learn?](#discovery-what-did-i-learn)
   - [Recommendations](#recommendations)
   - [Action Plan](#action-plan)
 








<br>
<br>
<br>

# Objective


What is the key pain point?

The main pain point in analyzing card brand performance is insufficient visibility into KPIs related to credit limits and card distribution. Stakeholders may struggle to quickly derive actionable insights from complex datasets due to a lack of intuitive reporting tools or comprehensive analyses that can highlight trends and correlations.


What is the ideal solution?

A dedicated analytics dashboard specifically designed for monitoring card brand performance. This platform would provide:
- Real-time visualizations of total credit limits across all cards.
-	Automated calculations for average credit limits per client with easy-to-read graphs.
-	Unique client tracking to understand market penetration better.
-	Comprehensive breakdowns by card brands and types with interactive charts illustrating quantity distributions.
-	Insights into how different brands contribute to overall credit limits through pie charts or bar graphs.
-	Correlation analysis tools that allow users to explore relationships between card types and their respective limits dynamically.




<br>

## User story

As a data analyst in our finance department, I want an interactive dashboard that gives me insights into our card brand performance so that I can provide management with data-driven recommendations based on real-time metrics



<br>
<br>

# Data Source

Where is the data coming from? The data is sourced from Kaggle, [clique aqui](https://www.kaggle.com/datasets/computingvictor/transactions-fraud-datasets)


<br>
<br>
<br>


# Stages

- Design
-	Development
-	Data Cleaning
-	Analysis




<br>
<br>
<br>





# Design

What should the dashboard contain based on the requirements provided?


To understand what it should contain, I need to figure out what questions the dashboard needs to answer:
1. What is the total credit limit across all cards?

2. What is the average credit limit for all clients?

3. How many unique clients hold cards within our system?

4. Which card brands have the most cards issued?

5: What is the distribution of different card types in terms of quantity?

6: Is there a correlation between card types and their respective limits?


<br>
<br>
<br>

## Tools

| Tool       | Purpose                                                  |
| -----------|----------------------------------------------------------|
| Excel      |	Exploring the data                                      |
| SQL Server |	Cleaning, testing, and analyzing the data               |
| Power BI   |	Visualizing the data via interactive dashboard          |
| Power Point| Create a modern and creative Dashboard layouts           |
| GitHub	   | Hosting the project documentation and version control    |


<br>
<br>



## Dashboard Mockup

![Dashboard mockup](https://github.com/user-attachments/assets/c273c1e5-81c4-485c-a677-fe9b3ca8055c)






Some of the data visuals that may be appropriate in this case include:

-	Tables
-	Treemap
-	Donuts chart
-	Pie chart
-	Cards
-	Starcked bar/column chart











<br>
<br>
<br>




# Development



What's the general approach in creating this solution from start to finish?

-	Get the data
-	Explore the data in Excel
-	Load the data into SQL Server
-	Clean and analyse the data with SQL
-	Visualize the data in Power BI
-	Create a layout for the dashboard in Power Point
-	Generate the findings based on the insights
-	Write the documentation
-	Publish the data to GitHub Page

<br>
<br>


## Data Exploration Notes


The Excel Data from Kaggle was extremely cleaned. There are no bugs, errors, inconsistencies or weird and corrupted characters.
There are at least 6 columns that contain the data we need for this analysis, which signals we have everything we need from the file without needing to contact the client for any more data.
Some of the other columns has a different type of data such as CVV or expires dates - we need to confirm if these columns are needed, and if so, we need to address them.
We have more data than we need, so some of these columns would need to be removed


The aim is to refine our dataset to ensure it is structured and ready for analysis.
The cleaned data should meet the following criteria and constraints:
- Only relevant columns should be retained.
-	All data types should be appropriate for the contents of each column.
-	No column should contain null values, indicating complete data for all records.


What steps are needed to clean and shape the data into the desired format?
1.	Remove unnecessary columns by only selecting the ones we need
2.	Verify all the data type and make sure they are correct

<br>
<br>

## DATA CLEANING

The cleaned data should meet the following criteria and constraints:
-	Only relevant columns should be retained.
-	All data types should be appropriate for the contents of each column.
-	No column should contain null values, indicating complete data for all records.




<br>
<br>





## Transform the Data and Creating the SQL View
<br>


```sql
--- Select the required columns
--- Create a view to store the transformed data

CREATE VIEW view_financial_data AS

SELECT
      id,
      client_id,
      card_brand,
      card_type
      expires,
      credit_limit
FROM
     dbo.financial_data

``` 

<br>
<br>




## Data Type Check



<br>

```sql

--Select the required columns
--Checking the data type of each column

SELECT
      Column_name, data_type

FROM 
     INFORMATION_SCHEMA.COLUMNS

WHERE
     TABLE_NAME = 'view_financial_data'
```


<br>
<br>
<br>



# Visualization
<br>

### What does the dashboard look like?

![cardbranddashboardgif](https://github.com/user-attachments/assets/0c1b0e27-6dea-4971-8311-e8c890bdb50a)







## DAX Measures
<br>
<br>

```
1.Total Credit Limit:

total_limit = SUM(view_financial_data[credit_limit])


2.Average Credit Limit:

avg_limit = AVERAGE(view_financial_data[credit_limit])


3. Count of Unique Clients:

total_client = DISTINCTCOUNT(view_financial_data[client_id])


4. Count of Cards by Brand(for each brand):

cards_by_brand = COUNTROWS(GROUPBY('view_financial_data',view_financial_data[card_brand]))


5. Count of Cards by Type (for each type):

cards_by_type = COUNTROWS(GROUPBY('view_financial_data',view_financial_data[card_type]))

6. Proportion of Each Card Brand's Total Limits:

brand_proportion(%) = DIVIDE(SUM(view_financial_data[credit_limit]),[total_limit],0)*100
```


<br>
<br>


# ANALYSIS
<br>

Below are the key questions that need to be addressed by our finance department

-	What is the average credit limit across different card brands or card types?
-	Are there notable differences in average credit limits among various client segments?
-	Client Demographics and Segmentation. How many clients do we have per card brand?
-	What is the distribution of different card types among clients?


<br>
<br>


## Discovery, what did I learn?

### I discovered that

1. The total credit limit across all cards is R$88.18M.
2. The average credit limit for all clients is R$14.35k.
3. The total number of unique clients holding cards is 2,000.
4. Mastercard has the most cards issued (1,665).
5. The distribution of different card types in terms of quantity:
Debit: 48.3%,
Credit: 37.26%,
Debit (Prepaid): 14.44%
7. Mastercard has the highest total limit (R$47.04M), while Discover has the lowest (R$2.26M).
8. Among card types, Debit has the highest total limit (R$65.16M) and Debit (Prepaid) has the lowest (R$37.25k).
9. There is a correlation between card types and their respective limits:
Debit: R$65.16M,
Credit: R$22.99M,
Debit (Prepaid): R$37.25k

<br>
<br>

## Recommendations
<br>

What do I recommend based on the insights gathered?

- Since Debit cards hold the highest total limit (R$65,16M) and are the most popular (48.3%), efforts should be concentrated on promoting and enhancing these card offerings. Maybe this could include new features, rewards programs, or improved customer service for debit card users.
-	Even though Credit cards are behind Debit in both total limit and quantity, they still represent a substantial portion of the market (37.26%). Consider strategies to increase credit card adoption, such as lower interest rates, introductory offers, or tailored credit limits based on user profiles.
-	While Debit (Prepaid) cards have the lowest total limit and quantity, they cater to a niche market that could be further developed. Explore opportunities to attract more customers to this card type through targeted marketing campaigns, partnerships with employers, or benefits for budgeting and financial management.
-	As Mastercard holds the highest number of issued cards and total limit, reinforcing its market leadership through exclusive benefits, marketing campaigns, and strategic partnerships could solidify its dominance.
-	Discover has the lowest total limit (R$2,26M), indicating a need to boost its appeal. Evaluate the reasons behind its lower adoption and implement strategies such as more competitive rewards programs, partnerships, and customer education initiatives to improve its market share.
-	Utilize the data to segment your customer base and offer personalized recommendations and services. Tailor marketing strategies to different segments based on their card type preferences and spending behavior to enhance customer satisfaction and loyalty.


<br>
<br>


## Action Plan


### What course of action should the finance department take?

1.	They should focus on promoting Debit Cards,
2.	Expand Credit Card offerings
3.	Increase Prepaid Debit Card usage
4.	Enhance Mastercard’s Market Position
5.	Address Discover’s Low Market Share
6.	Customer Segmentation & Personalization

   <br>

### What steps do they take to implement the recommended decisions effectively?

They should develop a detailed plan that outlines the specific actions required to implement each recommendation, enhance existing card offerings and develop new products based on customer insights and conduct thorough market research to understand customer needs, preferences, and behavior.






