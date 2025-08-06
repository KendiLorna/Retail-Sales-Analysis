## **Novamart 2019 Sales Analysis**
#### Tools Used: Python (Pandas, os and datetime), Microsoft Excel
#### Project Type: Data Cleaning, Exploratory Data Analysis (EDA) & Dashboard Visualization
#### Role: Data Analyst

## Table of Contents
[Project Overview](#project-overview)

[Business Questions](#business-questions)

[Process](#process)

[Exploratory Data Analysis](#exploratory-data-analysis)

[Dashboard](#novamart-sales-2019-dashboard)

[Key Insights](#key-insights)

[Recommendations](#recommendations)

## Project Overview

I analyzed 2019 sales data for a fictional retail company, Novamart, to uncover trends, performance drivers, and regional insights.
The goal was to answer the manager's business questions to inform strategic decision-making and identify opportunities for revenue growth and operational efficiency.

## Business Questions

1.	Which states and cities generate the highest and lowest sales, and what factors contribute to these differences?
2.	What are the key sales trends over time, including peak days, seasons, and hours of high activity?
3.	Which products drive the most revenue, and are there patterns in how customers buy them (e.g., frequently bought together, in bulk, by region)?
4.	How does customer purchasing behavior vary across locations in terms of order size, frequency, and product mix?
5.	Where are the biggest growth opportunities based on sales trends, underperforming regions, or untapped customer segments?

## Process
### Data Cleaning & Preparation (Python)

#### Imported and merged multiple files(monthly sales) into one dataset.

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
*Read in and displayed top data rows*

<img width="1484" height="333" alt="Screenshot 2025-08-05 102328" src="https://github.com/user-attachments/assets/1e5d650c-6a1a-47c8-ad3c-ad60501b1126" />

#### Found and deleted null values
```python
nan_df = all_data[all_data.isna().any(axis = 1)]
nan-df.head()
```
*NAN values*

<img width="1473" height="394" alt="Screenshot 2025-08-05 101915" src="https://github.com/user-attachments/assets/a0d06219-4d5d-4436-84d2-53539841550e" />

```python
all_data = all_data.dropna(how = 'all')
all_data.head()
```
#### Found duplicates
```pyhton
duplicates = all_data[all_data.duplicated(keep=False)]
duplicates
```
*Duplicates*

<img width="1466" height="643" alt="Screenshot 2025-08-05 100234" src="https://github.com/user-attachments/assets/99924af5-4127-4828-88e6-8ddcdc1de333" />

#### Dropped duplicates
```python
all_data.drop_duplicates()
```
#### Found and deleted Order Date values that had 'Or'
```python
all_data = all_data[all_data['Order Date'].str[0:2] != 'Or']
all_data
```
#### Changed incorrect data types.
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
#### Created a new column for total price
```python
all_data.insert(all_data.columns.get_loc("Price Each") + 1,"Total Price",all_data["Quantity Ordered"] * all_data["Price Each"])
```
*Cleaned Data*

<img width="1484" height="539" alt="Screenshot 2025-08-05 101548" src="https://github.com/user-attachments/assets/a405156c-ee94-4f18-aee0-384b3359fa4d" />

#### Converted cleaned data to csv for further exploration in Excel
```python
all_data.to_csv('Sales_2019.csv')
```
### Data Cleaning & Preparation (Excel)

I imported the csv file and transformed it in Power Query.

I created new columns by splitting the purchase address column into City & State(due to similar cities in different states)and State.

Street name and final Purchase address were not necessary for this analysis and were deleted.

<img width="663" height="663" alt="Screenshot 2025-08-05 115724" src="https://github.com/user-attachments/assets/bdad3fd8-196c-4abf-bfe6-14fe21c540b7" />

I further extracted day and month name,their corresponding day and month number, hour of day.

*Month*
```excel
=TEXT([@[Order Date]],"MMM")
```
*Day*
```excel
=TEXT([@[Order Date]],"DDD")
```
*Weekday number*
```excel
=WEEKDAY([@[Order Date]],2)
```
*Hour of day*
```excel
=HOUR([@[Order Date]])
```
I further classified days as weekend or weekday and split the hours into morning, afternoon, evening and night.

*Classification into weekend/weekday*
```excel
=IF([@[Day Number]]>5,"Weekend","Weekday")
```
*Classification of part of day*
```excel
=IF(AND([@[Hour of Day]]>=6,[@[Hour of Day]]<12),"Morning",
IF(AND([@[Hour of Day]]>=12,[@[Hour of Day]]<18),"Afternoon",
IF(AND([@[Hour of Day]]>=18,[@[Hour of Day]]<24),"Evening","Night")))
```
I created a calendar table in PowerPivot and added it to the data model for slicer sorting.

<img width="1186" height="629" alt="Screenshot 2025-08-05 120244" src="https://github.com/user-attachments/assets/2a19c572-1fd8-4e45-b24c-d65db824c4d2" />

### Exploratory Data Analysis

+ A preliminary check using functions

<img width="1792" height="504" alt="Screenshot 2025-08-05 123354" src="https://github.com/user-attachments/assets/5610d575-91ad-4c42-92a9-7f996c8fbb04" />

+ Created pivot tables to analyze sales by time (hour, day, and month)

<img width="1725" height="551" alt="Screenshot 2025-08-05 124046" src="https://github.com/user-attachments/assets/cde6a1eb-2fdc-4bb4-b8fa-f42d36aba80b" />

+ Identified best-selling products, regions and summary statistics
  
<img width="1862" height="523" alt="Screenshot 2025-08-05 124005" src="https://github.com/user-attachments/assets/9ef9cfa4-b86a-4a57-8eeb-336e3eb547d3" />

### Data Visualization & Dashboarding (Excel)

+ Designed a dynamic dashboard using slicers for month, state, and product
Visuals included:
    - Revenue,customers,quantity,and order KPIs
    - Hourly and daily sales patterns
    - Regional performance-city-wise quantity and revenue 
    - Monthly trend with percentage monthly changes
    - Geographic map for state-level revenue

## Novamart Sales 2019 Dashboard

<img width="1886" height="858" alt="Screenshot 2025-08-04 152140" src="https://github.com/user-attachments/assets/24bb8577-47dc-418b-91de-285a06f9ebd0" />

[Watch Interactive video here](https://youtu.be/uJ_1ZPsxNYA)

[Find the Excel file here](https://drive.google.com/file/d/1mqmy9QK5DmJnbbX5WhVg0HSC9FWBTpsv/view?usp=sharing)

## Key Insights

### Overall metrics

- Revenue: $34.5M
- Orders: 186K
- Units Sold: 209K
- Customers: 178K

#### Top performing products
- By revenue: MacBook Pro Laptop – 23% of total revenue

- By volume: AAA Batteries (4-pack) – 15% of total quantity

### Time-Based Performance

#### Hourly and day part sales
Peak hours: 10 AM – 1 PM (Morning/Afternoon)

Best performing daypart: Afternoon (36%)

Lowest performing: Night (6%)

#### Daily sales
Highest sales days: Tuesday ($5.1M) and Wednesday ($5.0M)

Lowest: Thursday ($4.8M)

#### Monthly sales trend
Peak month: December ($4.6M) up by 44% from November

Consistent growth from Jan to April

Dip in June (-18%), July (-3%)

Recovery starts in October

### Geographical Sales Analysis

#### Top cities by revenue
- San Francisco, CA – $8.3M

- New York City, NY – $5.5M

- Atlanta, GA – $4.7M

#### Top cities by quantity
- San Francisco, CA – 50K units

- Los Angeles, CA - 33K units

- New York City, NY - 28K units

#### Top states by revenue
- California leads with $13.7M

- Other key contributors: New York, Georgia, Texas, Massachusetts

### Summary Insights
- Revenue peaked in December, driven by seasonal demand

- MacBook Pro led in revenue, while AAA batteries led in quantity sold

- Afternoons and Tuesdays were peak sales periods

- California was the top-performing state by a wide margin
## Recommendations
#### Focus on bestsellers
Prioritize marketing and stock availability for:
- MacBook Pro Laptop (high-ticket)
- AAA Batteries (4-pack) (high volume)

Bundle fast-moving accessories, such as AirPods, with big-ticket items to drive both revenue and volume.
#### Optimize timing for campaigns
- Schedule major email ads, flash sales, and online promotions between 10 AM to 1 PM, targeting the Afternoon peak.
- Tuesday and Wednesday are the best days to launch new campaigns or limited-time offers.
#### Investigate mid-year sales slump
Investigate the June to August drop in sales:
- Was it due to inventory issues, market saturation, or lack of promotions?
- Consider mid-year clearance sales or back-to-school bundles.
#### Strengthen Market Presence in Key Regions
Focus on California, New York, and Georgia with regional campaigns.

Expand presence in underperforming areas like Seattle, WA, which had low revenue ($0.4M) despite being a large city.
#### Segment campaigns by day part
- Morning: Productivity tools and electronics
- Afternoon: Lifestyle and personal accessories
- Evening: Entertainment and family bundles
#### Incentivize repeat purchases
- With 178K customers, focus on loyalty programs to boost retention.
- Offer exclusive deals to repeat buyers and upsell based on previous purchases.


