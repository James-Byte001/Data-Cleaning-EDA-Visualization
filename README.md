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

## EDA (Exploratory Data Analysis) and Visualization

1. Data Overview

**Goal**: Understand the general structure of the dataset and check for any immediate issues.

- **Summary Statistics**: Get a quick overview of the dataset's numerical columns.
- **Data Types and Null Values**: Identify columns with missing or incorrect data types.
- **Count Unique Values**: Use `.nunique()` to get the count of unique entries for categorical columns such as Category, Segment, Region, Sub-Category, etc.

  ```python
  df.describe()

  df.info()

  df.isnull().sum()

  df.nunique()

2. Sales Perfomance

   - **Top Performing Categories/Sub-Categories**

In this section, I analyze the sales distribution across different product **Categories** and **Sub-Categories** to identify top-performing areas.

- **Sales by Category**

  ```python
  import matplotlib.pyplot as plt
  import seaborn as sns

  category_sales = df.groupby('Category')['Sales'].sum().sort_values(ascending=False)

  colors = ['skyblue', 'red', 'navy']

  plt.figure(figsize=(10, 6))

  category_sales.head(3).plot(kind='bar', color=colors)

  plt.title("Sales by Category")
  plt.xlabel("Category")
  plt.ylabel("Total Sales")
  plt.xticks(rotation=45)
  plt.show()

![Screenshot (41)](https://github.com/user-attachments/assets/a9712e6a-d1d7-473d-8a40-e65f9f96b93f)


**Top Performing Sub-Categories**

In this section, I analyze the sales distribution across the top 10 **Sub-Categories** to identify the highest-performing areas.

    ```python
    import matplotlib.pyplot as plt

    subcategory_sales = df.groupby('Sub-Category')['Sales'].sum().sort_values(ascending=False).head(10)

    colors = ['skyblue', 'red', 'green', 'orange', 'purple', 'pink', 'brown', 'yellow', 'gray', 'lightblue']

    plt.figure(figsize=(10, 6))

    subcategory_sales.plot(kind='bar', color=colors)

    plt.title("Top 10 Sales by Sub-Category")
    plt.xlabel("Sub-Category")
    plt.ylabel("Total Sales")
    plt.xticks(rotation=45)
    plt.show()
    
![Screenshot (42)](https://github.com/user-attachments/assets/4798bd3f-7068-469f-b7ef-92b7179dd9f6)


   



