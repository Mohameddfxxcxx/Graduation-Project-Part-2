## E-Commerce Customer Segmentation and Transaction Analysis

## Project Overview
This project aims to analyze and segment e-commerce customers based on their transaction data, using various data analysis and clustering techniques. We explored different clustering methods like DBSCAN and K-Means to segment customers into groups based on their transaction patterns. Additionally, we performed various analyses and visualized key insights into customer behavior, such as transaction value distribution, branch engagement, and customer demographics.

## Data Sources
The project uses multiple datasets related to customers, transactions, branches, and merchants. The structure of the main datasets is as follows:

1. Transactions Data: Contains details of customer transactions such as transaction ID, customer ID, transaction date, transaction status, coupon usage, and branch/merchant information.
   Key columns:

transaction_id

customer_id

transaction_date

transaction_value

branch_id

merchant_id

gender_name


2. Customers Data: Contains information about customer demographics like join date and gender.

Key columns:

customer_id

join_date

gender_id

gender_name




3. Branches Data: Contains branch information associated with the transactions.

Key columns:

branch_id

merchant_id



4. Merchants Data: Contains merchant information related to the transactions.

Key columns:

merchant_id

merchant_name



## Steps Taken
1. Data Preprocessing

Merged transaction, branch, merchant, and customer data into a unified dataset called transactions_full.

Handled missing data and ensured consistency in columns.


2. Feature Engineering

Aggregated Customer Data: Created an aggregated view of customer transactions, including:

transaction_count: Total number of transactions per customer.

branch_count: Number of branches visited per customer.

merchant_count: Number of merchants used per customer.

dbscan_cluster: Cluster label generated from the DBSCAN clustering algorithm.


Transaction Analysis: Computed the total and average transaction values for each customer.



3. Clustering

DBSCAN: Used DBSCAN (Density-Based Spatial Clustering) to segment customers based on their transaction behavior. The key features used for clustering included transaction_count, branch_count, and merchant_count.

4. Visualizations

We used Seaborn and Matplotlib to create various plots that offer insight into the dataset, including:

Scatter Plot: Visualized the number of transactions versus total transaction value, color-coded by DBSCAN clusters.

Bar Plot: Showed the distribution of branches visited by customers.

Line Plot: Illustrated the number of new customers joining the platform over time.


## Key Insights

1. Customer Segmentation: Using DBSCAN, we segmented customers into various clusters based on their transaction behavior, revealing different levels of customer engagement (e.g., highly active vs. inactive customers).


2. Transaction Patterns: Analysis of transaction values showed significant variation in spending behavior across customer clusters, with some clusters having much higher average transaction values than others.


3. Branch and Merchant Engagement: Customers were found to visit multiple branches and merchants, with a notable concentration of visits to certain branches.


## Code Snippets
1. Data Preprocessing
python<br>transactions_full = transactions.merge(customers, on='customer_id', how='left')<br>                                .merge(branches, on='branch_id', how='left')<br>                                .merge(merchants, on='merchant_id', how='left')<br> |

2. Feature Engineering
python<br>customer_aggregated = transactions_full.groupby('customer_id').agg(<br>    transaction_count=('transaction_id', 'count'),<br>    branch_count=('branch_id', 'nunique'),<br>    merchant_count=('merchant_id', 'nunique')<br>).reset_index()<br> |

3. Clustering
python<br>from sklearn.cluster import DBSCAN<br><br>dbscan = DBSCAN(eps=0.5, min_samples=5)<br>customer_aggregated['dbscan_cluster'] = dbscan.fit_predict(customer_aggregated[['transaction_count', 'branch_count', 'merchant_count']])<br> |

4. Visualization
python<br>import matplotlib.pyplot as plt<br>import seaborn as sns<br><br># Scatter Plot of Transactions vs. Total Transaction Value<br>plt.figure(figsize=(10, 6))<br>sns.scatterplot(x='transaction_count', y='total_transaction_value', data=customer_aggregated, hue='dbscan_cluster', palette='viridis')<br>plt.title('Transactions vs Transaction Value per Customer')<br>plt.xlabel('Number of Transactions')<br>plt.ylabel('Total Transaction Value')<br>plt.show()<br> |

python<br># Distribution of Branches per Customer<br>plt.figure(figsize=(10, 6))<br>sns.barplot(x=customer_aggregated['branch_count'].value_counts().index, y=customer_aggregated['branch_count'].value_counts().values, color='blue')<br>plt.title('Distribution of Branches per Customer')<br>plt.xlabel('Number of Branches')<br>plt.ylabel('Number of Customers')<br>plt.show()<br> |


python<br># Number of Customers by Join Date<br>customer_join_date = customer_aggregated.groupby(customer_aggregated['join_date'].dt.to_period('M'))['customer_id'].count().reset_index()<br>plt.figure(figsize=(10, 6))<br>sns.lineplot(x='join_date', y='customer_id', data=customer_join_date, marker='o', color='purple')<br>plt.title('Number of Customers by Join Date')<br>plt.xlabel('Join Date (Month)')<br>plt.ylabel('Number of Customers')<br>plt.show()<br> |
| *Transaction Value Distribution by Gender* | python<br># Transaction Value Distribution by Gender<br>plt.figure(figsize=(10, 6))<br>sns.boxplot(x='gender_name', y='transaction_value', data=transactions_full, palette='muted')<br>plt.title('Transaction Value Distribution by Gender')<br>plt.xlabel('Gender')<br>plt.ylabel('Transaction Value')<br>plt.show()<br> |




## Future Work

1. Hierarchical Clustering: Implement additional clustering techniques such as Hierarchical Clustering for comparison with DBSCAN.


2. Deeper Customer Segmentation: Explore more features for customer segmentation, such as purchase frequency and the use of coupons.


3. Time Series Analysis: Perform detailed time series analysis on transaction dates to explore seasonality and trends in customer activity.



## Requirements

Python Libraries:

pandas

numpy

matplotlib

seaborn

sklearn

Make sure to install the required libraries using:

pip install pandas numpy matplotlib seaborn scikit-learn



## Author

This project was developed by [Mohamed El-sadek]. It is designed for e-commerce businesses looking to improve customer segmentation and better understand transaction data patterns.

