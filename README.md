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

## Top 10 Products by Sales

In this section, I analyze the sales distribution across the top 10 **Products** to identify the highest-performing items. By grouping the data by **Product Name** and summing the total sales, I highlight the products that contribute most to overall sales performance.

    ```python
    import pandas as pd
    import matplotlib.pyplot as plt
    import seaborn as sns

    top_products = df.groupby('Product Name')['Sales'].sum().reset_index()
    top_products_sorted = top_products.sort_values(by='Sales', ascending=False)
    top_10_products = top_products_sorted.head(10)

    custom_colors = sns.color_palette("Set1", n_colors=10)

    plt.figure(figsize=(10,6))
    sns.barplot(x='Sales', y='Product Name', data=top_10_products, palette=custom_colors)

    plt.title('Top 10 Products by Sales', fontsize=16)
    plt.xlabel('Total Sales', fontsize=12)
    plt.ylabel('Product Name', fontsize=12)

    plt.tight_layout()
    plt.show()
   ![Screenshot (43)](https://github.com/user-attachments/assets/703ada1b-52e5-4a6e-8a98-c5b43890a4d5)


 ## Sales Trend by Year

In this section, I visualize the sales trend over the years by grouping the data by **Year** and summing the total sales for each year. The following code generates a line plot to show how sales have varied across different years.

    ```python
    import pandas as pd
    import matplotlib.pyplot as plt
    import seaborn as sns

    df['Order Date'] = pd.to_datetime(df['Order Date'])
    df['Year'] = df['Order Date'].dt.year
    sales_by_year = df.groupby('Year')['Sales'].sum().reset_index()

    plt.figure(figsize=(10,6))
    sns.lineplot(x='Year', y='Sales', data=sales_by_year, marker='o', color='r')

    plt.title('Sales Trend by Year', fontsize=16)
    plt.xlabel('Year', fontsize=12)
    plt.ylabel('Total Sales', fontsize=12)

    plt.tight_layout()
    plt.show()
    
![Screenshot (44)](https://github.com/user-attachments/assets/d6cca88f-e1f5-47d3-96bf-81c1aaca7f0c)


## Sales by Region

In this section, I analyze the total sales for each **Region** to identify which regions have the highest sales performance. The following code generates a bar plot to show the sales distribution across different regions.

    ```python
    import pandas as pd
    import matplotlib.pyplot as plt
    import seaborn as sns

    df['Region'] = df['Region'].astype(str)

    sales_by_region = df.groupby('Region')['Sales'].sum().reset_index()

    custom_colors = sns.color_palette("Set1", n_colors=10)

    plt.figure(figsize=(10,6))
    sns.barplot(x='Region', y='Sales', data=sales_by_region, palette=custom_colors)

    plt.title('Sales by Region', fontsize=16)
    plt.xlabel('Region', fontsize=12)
    plt.ylabel('Total Sales', fontsize=12)

    plt.tight_layout()
    plt.show()

  ![Screenshot (46)](https://github.com/user-attachments/assets/51771276-9bb0-4717-8eb4-5906202fb377)



## Top 10 States by Sales

In this section, I analyze the total sales for the **top 10 states** based on overall sales performance. The following code generates a bar plot to visualize the sales distribution for the top 10 states.

    ```python
    import pandas as pd
    import matplotlib.pyplot as plt
    import seaborn as sns

    sales_by_state = df.groupby('State')['Sales'].sum().reset_index()
    top_10_states = sales_by_state.nlargest(10, 'Sales')

    plt.figure(figsize=(10,6))
    sns.barplot(x='Sales', y='State', data=top_10_states, palette='Set1')

    plt.title('Top 10 States by Sales', fontsize=16)
    plt.xlabel('Total Sales', fontsize=12)
    plt.ylabel('State', fontsize=12)

    plt.tight_layout()
    plt.show()


![Screenshot (54)](https://github.com/user-attachments/assets/e287a19e-480f-4193-9158-591806722c32)

    
## Sales Performance by Months of Year

In this section, we analyze the sales performance over the months of each year. The bar plot below visualizes the total sales for each month, aggregated across all years, helping us identify trends and patterns in the sales data.

    ```python
    import pandas as pd
    import matplotlib.pyplot as plt
    import seaborn as sns

    df['Order Date'] = pd.to_datetime(df['Order Date'])
    df['Month'] = df['Order Date'].dt.month
    sales_by_month = df.groupby('Month')['Sales'].sum().reset_index()

    plt.figure(figsize=(10,6))
    sns.barplot(x='Month', y='Sales', data=sales_by_month, palette='Set1')

    plt.title('Sales Performance by Months of Year', fontsize=16)
    plt.xlabel('Month', fontsize=12)
    plt.ylabel('Total Sales', fontsize=12)
    plt.xticks(ticks=range(12), labels=['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'])

    plt.tight_layout()
    plt.show()
    
![Screenshot (48)](https://github.com/user-attachments/assets/9a92ddac-8ef2-42dd-a063-c55936e813da)


## Creating the `Delayed_Order` Column

In this section, we will create a new column called **`Delayed_Order`** to identify whether an order is delayed or not. An order is considered delayed if the **Ship Date** is more than 2 days after the **Order Date**.

    ```python
    import pandas as pd

    df['Order Date'] = pd.to_datetime(df['Order Date'])
    df['Ship Date'] = pd.to_datetime(df['Ship Date'])

    df['Delayed_Order'] = df['Ship Date'] > df['Order Date'] + pd.Timedelta(days=4)

    df['Delayed_Order'] = df['Delayed_Order'].map({True: 'Yes', False: 'No'})

    print(df[['Order ID', 'Order Date', 'Ship Date', 'Delayed_Order']].head())


 ## Distribution of Delayed vs On-Time Orders

In this section, we visualize the distribution of delayed orders versus on-time orders using a pie chart. The chart below helps us understand the proportion of orders that are delayed compared to those that are shipped on time.

    ```python
    import matplotlib.pyplot as plt
    import seaborn as sns

    delayed_orders = df[df['Delayed_Order'] == 'Yes']
    on_time_orders = df[df['Delayed_Order'] == 'No']

    delayed_count = len(delayed_orders)
    on_time_count = len(on_time_orders)

    plt.figure(figsize=(6,6))
    plt.pie([on_time_count, delayed_count], labels=['On-Time Orders', 'Delayed Orders'], 
        autopct='%1.1f%%', startangle=90, colors=['#1f77b4', '#ff7f0e'])

    plt.title('Distribution of Delayed vs On-Time Orders', fontsize=16)

    plt.show()
    
   ![Screenshot (49)](https://github.com/user-attachments/assets/a5467af1-1158-4610-ae8e-b738ecc52dc5)


## Delayed Orders Over Time

In this section, we analyze the distribution of delayed orders over time by year and month. The line graph below shows the number of delayed orders for each month, helping us identify any seasonal trends.

    ```python
    import pandas as pd
    import matplotlib.pyplot as plt
    import seaborn as sns

    df['Order Date'] = pd.to_datetime(df['Order Date'])
    df['Ship Date'] = pd.to_datetime(df['Ship Date'])

    df['Delayed_Order'] = df['Ship Date'] > df['Order Date'] + pd.Timedelta(days=3)
    df['Delayed_Order'] = df['Delayed_Order'].map({True: 'Yes', False: 'No'})

    df['Year'] = df['Order Date'].dt.year
    df['Month'] = df['Order Date'].dt.month

    delayed_orders_by_time = df[df['Delayed_Order'] == 'Yes'].groupby(['Year', 'Month']).size().reset_index(name='Delayed Count')

    plt.figure(figsize=(10,6))
    sns.lineplot(data=delayed_orders_by_time, x='Month', y='Delayed Count', hue='Year', marker='o', palette='coolwarm')
    plt.title('Delayed Orders Over Time', fontsize=16)
    plt.xlabel('Month', fontsize=12)
    plt.ylabel('Delayed Orders Count', fontsize=12)
    plt.xticks(ticks=range(1, 13), labels=['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'])
    plt.show()
    
 ![Screenshot (50)](https://github.com/user-attachments/assets/67b95ae8-686a-4d5f-8eed-a49fd5fcfc46)
   
 ## Distribution of Delayed Orders by Product

In this section, we focus on the distribution of delayed orders across different products. The bar plot below shows the top 10 products that have the highest number of delayed orders.

    ```python
    import pandas as pd
    import matplotlib.pyplot as plt
    import seaborn as sns

    df['Order Date'] = pd.to_datetime(df['Order Date'])
    df['Ship Date'] = pd.to_datetime(df['Ship Date'])

    df['Delayed_Order'] = df['Ship Date'] > df['Order Date'] + pd.Timedelta(days=3)
    df['Delayed_Order'] = df['Delayed_Order'].map({True: 'Yes', False: 'No'})

    delayed_orders_by_product = df[df['Delayed_Order'] == 'Yes'].groupby('Product Name').size().reset_index(name='Delayed Count')

    top_delayed_products = delayed_orders_by_product.sort_values(by='Delayed Count', ascending=False).head(10)

    plt.figure(figsize=(12,8))


    custom_colors = sns.color_palette("husl", n_colors=10)

    sns.barplot(x='Delayed Count', y='Product Name', data=top_delayed_products, palette=custom_colors)
    plt.title('Top 10 Products with Delayed Orders', fontsize=16)
    plt.xlabel('Delayed Orders Count', fontsize=12)
    plt.ylabel('Product Name', fontsize=12)
    plt.tight_layout()
    plt.show()
    
![Screenshot (51)](https://github.com/user-attachments/assets/1dfe58a6-0e28-4490-a90c-8dc6c808d81b)


## Delayed Orders by Region

In this section, we analyze the distribution of delayed orders across different regions. The bar plot below shows how many delayed orders occurred in each region.

    ```python
    import pandas as pd
    import matplotlib.pyplot as plt
    import seaborn as sns

    df['Order Date'] = pd.to_datetime(df['Order Date'])
    df['Ship Date'] = pd.to_datetime(df['Ship Date'])

    df['Delayed_Order'] = df['Ship Date'] > df['Order Date'] + pd.Timedelta(days=3)
    df['Delayed_Order'] = df['Delayed_Order'].map({True: 'Yes', False: 'No'})

    delayed_orders_by_region = df[df['Delayed_Order'] == 'Yes'].groupby('Region').size().reset_index(name='Delayed Count')

    # Sorting by the number of delayed orders in descending order
    delayed_orders_by_region_sorted = delayed_orders_by_region.sort_values(by='Delayed Count', ascending=False)

    plt.figure(figsize=(10,6))

    # Use a beautiful color palette
    beautiful_colors = sns.color_palette("magma", n_colors=len(delayed_orders_by_region_sorted))

    sns.barplot(x='Region', y='Delayed Count', data=delayed_orders_by_region_sorted, palette=beautiful_colors)
    plt.title('Delayed Orders by Region', fontsize=16)
    plt.xlabel('Region', fontsize=12)
    plt.ylabel('Delayed Orders Count', fontsize=12)
    plt.xticks(rotation=45)

    plt.tight_layout()
    plt.show()
![Screenshot (53)](https://github.com/user-attachments/assets/582d0c50-fc57-4c60-b3bf-f976d651bafe)

## Sales by Customer Segment

In this section, we examine the total sales performance across various customer segments. The bar plot below shows the total sales for each customer segment.

    ```python
    import pandas as pd
    import matplotlib.pyplot as plt
    import seaborn as sns

    df['Sales'] = pd.to_numeric(df['Sales'], errors='coerce')

    sales_by_segment = df.groupby('Segment')['Sales'].sum().reset_index()

    plt.figure(figsize=(10,6))

    # Use a unique color palette
    custom_palette = sns.color_palette("viridis", n_colors=len(sales_by_segment))

    sns.barplot(x='Segment', y='Sales', data=sales_by_segment, palette=custom_palette)
    plt.title('Total Sales by Customer Segment', fontsize=16)
    plt.xlabel('Customer Segment', fontsize=12)
    plt.ylabel('Total Sales', fontsize=12)

    plt.tight_layout()
    plt.show()

![Screenshot (52)](https://github.com/user-attachments/assets/b2223b10-85d5-4471-b6c1-dbcea0e5b37f)



## Top 10 States with Delayed Orders

In this section, we focus on the states with the highest number of delayed orders. The bar chart below visualizes the distribution of delayed orders by state, helping us identify which states have the most significant delays in order shipments.

    ```python
    import pandas as pd
    import matplotlib.pyplot as plt
    import seaborn as sns

    df['Order Date'] = pd.to_datetime(df['Order Date'])
    df['Ship Date'] = pd.to_datetime(df['Ship Date'])

    df['Delayed_Order'] = df['Ship Date'] > df['Order Date'] + pd.Timedelta(days=3)
    df['Delayed_Order'] = df['Delayed_Order'].map({True: 'Yes', False: 'No'})

    delayed_orders_by_state = df[df['Delayed_Order'] == 'Yes'].groupby('State').size().reset_index(name='Delayed Count')

    delayed_orders_by_state_sorted = delayed_orders_by_state.sort_values(by='Delayed Count', ascending=False).head(10)

    plt.figure(figsize=(10,6))

    unique_colors = sns.color_palette("Set3", n_colors=len(delayed_orders_by_state_sorted))

    sns.barplot(x='State', y='Delayed Count', data=delayed_orders_by_state_sorted, palette=unique_colors)
    plt.title('Top 10 States with Delayed Orders', fontsize=16)
    plt.xlabel('State', fontsize=12)
    plt.ylabel('Delayed Orders Count', fontsize=12)
    plt.xticks(rotation=45)

    plt.tight_layout()
    plt.show()

![Screenshot (56)](https://github.com/user-attachments/assets/043702ee-a0e5-4a2f-a535-76aaea8c25ee)


## Distribution of Shipment Modes

In this section, I analyze the distribution of different **shipment modes** to understand which ones are used the most. The following code generates a bar plot to visualize the frequency of each shipment mode.

    ```python
    import matplotlib.pyplot as plt
    import seaborn as sns

    custom_colors = ["#FF6F61", "#6B5B95", "#88B04B", "#F7CAC9"]

    plt.figure(figsize=(8,5))
    ax = sns.countplot(x=df['Ship Mode'], palette=custom_colors)

    for p in ax.patches:
    ax.annotate(f'{p.get_height()}', (p.get_x() + p.get_width() / 2, p.get_height()), 
                ha='center', va='bottom', fontsize=12, fontweight='bold')

    plt.title('Distribution of Shipment Modes', fontsize=14, fontweight='bold')
    plt.xlabel('Shipment Mode', fontsize=12)
    plt.ylabel('Count of Orders', fontsize=12)

    plt.show()

    
  ![Screenshot (57)](https://github.com/user-attachments/assets/7f2fea58-cd44-4e69-a408-bf1cd6b5a79e)



## Shipment Mode vs. Delivery Time

In this section, I analyze the relationship between **Shipment Mode** and **Delivery Time** to understand how different shipping options impact the time taken for delivery. The following code generates a box plot to visualize the variation in delivery times across shipment modes.

    ```python
    import matplotlib.pyplot as plt
    import seaborn as sns

    plt.figure(figsize=(10,6))
    ax = sns.boxplot(x=df['Ship Mode'], y=df['Delivery_Time'], palette="Set2")

    plt.title('Shipment Mode vs. Delivery Time', fontsize=14, fontweight='bold')
    plt.xlabel('Shipment Mode', fontsize=12)
    plt.ylabel('Delivery Time (in Days)', fontsize=12)

    plt.show()

![Screenshot (58)](https://github.com/user-attachments/assets/f26e0243-f043-4c27-b409-51699ca71e49)

## Preferred Shipment Mode by Region

In this section, I analyze which **shipment modes are preferred in different regions**. This helps in understanding regional shipping trends and optimizing logistics. The following code generates a **stacked bar chart** to visualize the shipment mode distribution across regions.

    ```python
    import matplotlib.pyplot as plt
    import pandas as pd

    plt.figure(figsize=(12,6))
    region_shipmode = df.groupby(['Region', 'Ship Mode']).size().unstack()

    # Updated color palette with an additional unique color
    colors = ['skyblue', 'red', 'navy', 'gold']

    region_shipmode.plot(kind='bar', stacked=True, color=colors, figsize=(12,6))

    plt.title('Preferred Shipment Mode by Region', fontsize=14, fontweight='bold')
    plt.xlabel('Region', fontsize=12)
    plt.ylabel('Count of Orders', fontsize=12)
    plt.legend(title='Ship Mode')

    plt.xticks(rotation=45)
    plt.show()

![Screenshot (61)](https://github.com/user-attachments/assets/e190ab49-a3be-46c0-a1d4-408140f31f4a)

## Preferred Shipment Mode by Customer Segment

This analysis explores **which shipment modes are preferred by different customer segments**. Understanding these trends helps in tailoring **logistics and marketing strategies**.

    ```python
    import matplotlib.pyplot as plt
    import pandas as pd

    plt.figure(figsize=(12,6))
    segment_shipmode = df.groupby(['Segment', 'Ship Mode']).size().unstack()

    # Unique colors for each shipment mode
    colors = ['skyblue', 'red', 'navy', 'gold']

    segment_shipmode.plot(kind='bar', stacked=True, color=colors, figsize=(12,6))

    plt.title('Preferred Shipment Mode by Customer Segment', fontsize=14, fontweight='bold')
    plt.xlabel('Customer Segment', fontsize=12)
    plt.ylabel('Count of Orders', fontsize=12)
    plt.legend(title='Ship Mode')

    plt.xticks(rotation=45)
    plt.show()

    
![Screenshot (63)](https://github.com/user-attachments/assets/cb08758a-514b-4e81-ae6a-38c1417620ba)

## Preferred Shipment Mode by Top 10 States (Descending Order)

This analysis explores **which shipment modes are preferred by the top 10 states** with the highest number of orders, sorted in descending order. Understanding these preferences helps optimize logistics and delivery efficiency.

    ```python
    import matplotlib.pyplot as plt
    import pandas as pd

    # Get top 10 states by total orders in descending order
    top_10_states = df['State'].value_counts().head(10)
    filtered_df = df[df['State'].isin(top_10_states.index)]

    plt.figure(figsize=(12,6))
    state_shipmode = filtered_df.groupby(['State', 'Ship Mode']).size().unstack()

    # Sorting states based on total orders
    state_shipmode = state_shipmode.loc[top_10_states.index]

    # Unique colors for shipment modes
    colors = ['skyblue', 'red', 'navy', 'gold']

    state_shipmode.plot(kind='bar', stacked=True, color=colors, figsize=(12,6))

    plt.title('Preferred Shipment Mode by Top 10 States (Descending Order)', fontsize=14, fontweight='bold')
    plt.xlabel('State', fontsize=12)
    plt.ylabel('Count of Orders', fontsize=12)
    plt.legend(title='Ship Mode')

    plt.xticks(rotation=45)
    plt.show()

    
 
![Screenshot (64)](https://github.com/user-attachments/assets/92de9bee-b1cf-475c-b8e1-283b9e60ca3e)

 ## Preferred Shipment Mode by Month

This analysis explores **which shipment modes are preferred in different months of the year**. Identifying these trends can help businesses optimize logistics and ensure the availability of the most popular shipment modes during peak months.

    ```python
    import matplotlib.pyplot as plt
    import pandas as pd

    # Convert 'Order Date' to datetime format if not already done
    df['Order Date'] = pd.to_datetime(df['Order Date'])

    # Extract month name
    df['Month'] = df['Order Date'].dt.strftime('%B')

    # Define the order of months
    month_order = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']

    plt.figure(figsize=(12,6))
    month_shipmode = df.groupby(['Month', 'Ship Mode']).size().unstack()

    # Reorder months correctly
    month_shipmode = month_shipmode.reindex(month_order)

    # Unique colors for shipment modes
    colors = ['skyblue', 'red', 'navy', 'gold']

    month_shipmode.plot(kind='bar', stacked=True, color=colors, figsize=(12,6))

    plt.title('Preferred Shipment Mode by Month', fontsize=14, fontweight='bold')
    plt.xlabel('Month', fontsize=12)
    plt.ylabel('Count of Orders', fontsize=12)
    plt.legend(title='Ship Mode')

    plt.xticks(rotation=45)
    plt.show()

![Screenshot (66)](https://github.com/user-attachments/assets/c842ec3d-ac89-450b-9d99-d37197db762b)

## Orders by Day of the Week

This analysis explores **which days of the week have the highest number of orders**. Understanding this trend helps businesses plan for peak order days and optimize staffing or inventory accordingly.

    ```python
    import matplotlib.pyplot as plt
    import pandas as pd

    # Convert 'Order Date' to datetime format if not already done
    df['Order Date'] = pd.to_datetime(df['Order Date'])

    # Extract day of the week
    df['Day of Week'] = df['Order Date'].dt.day_name()

    # Define the order of days
    day_order = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']

    plt.figure(figsize=(10,6))
    orders_per_day = df['Day of Week'].value_counts().reindex(day_order)

    # Unique colors for each bar
    colors = ['skyblue', 'red', 'navy', 'gold', 'green', 'purple', 'orange']

    orders_per_day.plot(kind='bar', color=colors)

    plt.title('Orders by Day of the Week', fontsize=14, fontweight='bold')
    plt.xlabel('Day of the Week', fontsize=12)
    plt.ylabel('Count of Orders', fontsize=12)
 
    plt.xticks(rotation=45)
    plt.show()

    
![Screenshot (68)](https://github.com/user-attachments/assets/88f1b428-efc3-4a54-a00b-23de33aea0e1)

## Top 10 Customers by Sales

This analysis explores **the top 10 customers based on total sales**. Identifying high-value customers helps businesses develop personalized marketing strategies and loyalty programs.

    ```python
    import matplotlib.pyplot as plt
    import pandas as pd

    # Get top 10 customers by total sales
    top_10_customers = df.groupby('Customer Name')['Sales'].sum().nlargest(10)

    plt.figure(figsize=(12,6))

    # Unique colors for each bar
    colors = ['skyblue', 'red', 'navy', 'gold', 'green', 'purple', 'orange', 'pink', 'brown', 'gray']

    top_10_customers.plot(kind='bar', color=colors)

    plt.title('Top 10 Customers by Sales', fontsize=14, fontweight='bold')
    plt.xlabel('Customer Name', fontsize=12)
    plt.ylabel('Total Sales', fontsize=12)

    plt.xticks(rotation=45)
    plt.show()

![Screenshot (70)](https://github.com/user-attachments/assets/33cb6cca-9a78-4f70-a544-550d6b4bf107)

## Top 10 Customers by Order Count

This analysis explores **the top 10 customers based on the number of orders placed**. Understanding which customers order most frequently helps in designing loyalty programs and targeted marketing campaigns.

    ```python
    import matplotlib.pyplot as plt
    import pandas as pd

    # Get top 10 customers by number of orders
    top_10_customers_orders = df['Customer Name'].value_counts().nlargest(10)

    plt.figure(figsize=(12,6))

    # Unique colors for each bar
    colors = ['skyblue', 'red', 'navy', 'gold', 'green', 'purple', 'orange', 'pink', 'brown', 'gray']

    top_10_customers_orders.plot(kind='bar', color=colors)

    plt.title('Top 10 Customers by Order Count', fontsize=14, fontweight='bold')
    plt.xlabel('Customer Name', fontsize=12)
    plt.ylabel('Number of Orders', fontsize=12)

    plt.xticks(rotation=45)
    plt.show()



![Screenshot (72)](https://github.com/user-attachments/assets/0cc2b9f7-6a15-45cb-a658-b9ab3bc2959d)























   



