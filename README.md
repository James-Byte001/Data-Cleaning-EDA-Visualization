# Sales_Analysis_Project

## Project Overview

This project focuses on analyzing sales data for an e-commerce company, with the goal of uncovering key insights related to customer behavior, sales performance, and operational efficiency. Through this analysis, I explore trends in sales over time, customer demographics, product performance, and shipping details, while cleaning and transforming raw data into actionable insights.
##  Dataset Description
The dataset consists of 9,800 rows of sales transactions for an e-commerce company. It includes information about customers, products, shipping details, and order performance. The dataset features 23 columns, such as Order ID, Customer ID, Product Category, Ship Mode, Delivery Time, and more. Some columns contain missing or irrelevant data, which were cleaned and transformed for analysis.
## Technologies and Tools Used

- **Programming Language**:
  - Python

- **Data Manipulation and Cleaning**:
  - Pandas, NumPy: For data cleaning and manipulation.
  
- **Exploratory Data Analysis (EDA)**:
  - Google Colab: For interactive analysis and collaboration.

- **Data Visualization**:
  - Matplotlib, Seaborn: For creating static visualizations (e.g., histograms, bar charts).

- **Version Control**:
  - Git, GitHub: For version control and collaboration.

- **Dependencies**:
  - pip, requirements.txt: For managing project dependencies.
 

## Key Objectives

- Clean and preprocess the data to ensure accuracy and consistency.
- Perform exploratory data analysis (EDA) to uncover insights into sales trends, delivery times, and customer behavior.
- Visualize sales data using various plots to identify key patterns.
- Investigate the relationship between delivery time, order processing, and sales performance.

## Data Cleaning and Manipulation in Google Colab

1. **Load the Data**:
   - Load your dataset from Google Drive after mounting it.

   ```python
   from google.colab import drive
   drive.mount('/content/drive')
   df = pd.read_csv('/content/drive/My Drive/Combined_Dataset.csv')

2. **Inspect the Data**:

   - Check the first few rows, data types, and missing values.

   ```python
   df.head()
   df.info()
   df.isnull().sum()

3.  Handle Missing Data

    - Fill or drop missing values in specific columns.

    ```python
    df['Postal Code'].fillna(df['Postal Code'].mode()[0], inplace=True)
    df.dropna(subset=['Postal Code'], inplace=True)


4. Convert Data Types

    - Convert date columns to datetime format.

    ```python
    df['Order Date'] = pd.to_datetime(df['Order Date'])
    df['Ship Date'] = pd.to_datetime(df['Ship Date'])

5. Remove Unnecessary Columns

    - Drop irrelevant or unnamed columns.

   ```python
   df.drop(columns=['Unnamed: 18', 'Unnamed: 19'], inplace=True)

6. Handle Duplicates

    - Remove duplicate rows.

   ```python
   df.drop_duplicates(inplace=True)

 7. Rename Columns

    - Rename columns for better clarity.

    ```python
    df.rename(columns={'Order ID': 'order_id', 'Ship Mode': 'ship_mode'}, inplace=True)


8. Create New Columns

    - Create new columns like `delivery_delay`.

    ```python
    df['delivery_delay'] = (df['Ship Date'] - df['Order Date']).dt.days






