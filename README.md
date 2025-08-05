### **Novamart 2019 Sales Analysis**
#### Tools Used: Python (Pandas, os and datetime), Microsoft Excel
#### Project Type:Data Cleaning, Exploratory Data Analysis (EDA) & Dashboard Visualization
#### Role: Data Analyst

### Table of Contents
##### Project Overview
##### Business Questions
##### Data Cleaning
##### Exploratory Data Analysis
##### Dashboard
##### Insights
##### Recommendations

### Project Overview:
I analyzed 2019 sales data for a fictional retail company, Novamart, to uncover trends, performance drivers, and regional insights.
The goal was to answer the manager's business questions to inform strategic decision-making and identify opportunities for revenue growth and operational efficiency.

### Business Questions:
1.	Which states and cities generate the highest and lowest sales, and what factors contribute to these differences?
2.	What are the key sales trends over time, including peak days, seasons, and hours of high activity?
3.	Which products drive the most revenue, and are there patterns in how customers buy them (e.g., frequently bought together, in bulk, by region)?
4.	How does customer purchasing behavior vary across locations in terms of order size, frequency, and product mix?
5.	Where are the biggest growth opportunities based on sales trends, underperforming regions, or untapped customer segments?

### Process:
### Data Cleaning & Preparation (Python):
+ Imported and merged multiple files(monthly sales) into one dataset.

```python
# Define the path
folder_path = r"D:\Kendi\Lorna DA\Datasets\Pandas Sales Analysis"

# Get all files in the folder
files = [file for file in os.listdir(folder_path) if file.endswith(".csv")]

# Create an empty DataFrame
all_months_data = pd.DataFrame()

# Loop through each file and read the data and add to empty dataframe
for file in files:
    file_path = os.path.join(folder_path, file)
    df = pd.read_csv(file_path)
    all_months_data = pd.concat([all_months_data, df], ignore_index=True)

# convert data to csv
all_months_data.to_csv('All_Data_2019.csv', index = False)
```
Read in and displayed top data rows<img width="1484" height="333" alt="Screenshot 2025-08-05 102328" src="https://github.com/user-attachments/assets/1e5d650c-6a1a-47c8-ad3c-ad60501b1126" />

+ Found and deleted null values
```python
nan_df = all_data[all_data.isna().any(axis = 1)]
nan-df.head()
```
NAN values<img width="1473" height="394" alt="Screenshot 2025-08-05 101915" src="https://github.com/user-attachments/assets/a0d06219-4d5d-4436-84d2-53539841550e" />

```python
all_data = all_data.dropna(how = 'all')
all_data.head()
```
+ Found duplicates
```pyhton
duplicates = all_data[all_data.duplicated(keep=False)]
duplicates
```
Duplicates present<img width="1466" height="643" alt="Screenshot 2025-08-05 100234" src="https://github.com/user-attachments/assets/99924af5-4127-4828-88e6-8ddcdc1de333" />

+ Dropped duplicates
```python
all_data.drop_duplicates()
```
+ Fixed Order Date column that had 'Or'
```python
all_data = all_data[all_data['Order Date'].str[0:2] != 'Or']
all_data
```
+ Changed incorrect data types.
``` python
all_data['Quantity Ordered'] = pd.to_numeric(all_data['Quantity Ordered'])
all_data['Price Each'] = all_data['Price Each'].astype('float')
all_data['Order Date'] = pd.to_datetime(all_data['Order Date'], format = "%m/%d/%y %H:%M")
all_data['Order ID'] = pd.to_numeric(all_data['Order ID'])
```
```python
all_data.dtypes
#new data types 
Product                     object
Quantity Ordered             int64
Price Each                 float64
Order Date          datetime64[ns]
Purchase Address            object
dtype: object
```
+ Created a new column for total price
```python
all_data.insert(all_data.columns.get_loc("Price Each") + 1,"Total Price",
   all_data["Quantity Ordered"] * all_data["Price Each"])
```
Cleaned Data<img width="1484" height="539" alt="Screenshot 2025-08-05 101548" src="https://github.com/user-attachments/assets/a405156c-ee94-4f18-aee0-384b3359fa4d" />

+ Converted cleaned data to csv for further exploration in Excel
```python
all_data.to_csv('Sales_2019.csv')
```
### Data Cleaning & Preparation (Excel):
I imported the csv file and transformed it in Power Query.

There,I created new columns by separating the purchase address column into city & state(due to similar cities in different states), state, and Street name and Zip code.

I further extracted day and month name,their correspomding day and month number, hour of day.

I further classified days as weekend or weekday and split the hours into morning, afternoon, evening and night.

I created a calendar table in PowerPivot and added it to the data model for slicer sorting.

### Exploratory Data Analysis (Excel):
•	Created pivot tables to analyze sales by time (hour, day, month)
•	Identified best-selling products and regions
•	Explored patterns in customer purchasing behavior
### Data Visualization & Dashboarding (Excel):
•	Designed a dynamic dashboard using slicers for month, state, and product
•	Visuals included:
o	Revenue and order KPIs
o	Hourly and daily sales patterns
o	Product and regional performance
o	Monthly trend with percentage monthly changes
o	Geographic map for state-level revenue
### Key Insights:
•	Revenue peaked in December, driven by seasonal demand
•	MacBook Pro led in revenue, while AAA batteries led in quantity sold
•	Afternoons and Tuesdays were peak sales periods
•	California was the top-performing state by a wide margin
### Impact & Recommendations:
•	Suggested targeted promotions during high-performing times (e.g., afternoons, mid-week)
•	Proposed bundling strategies and loyalty programs based on product and customer insights
•	Recommended regional campaign focus and inventory optimization

