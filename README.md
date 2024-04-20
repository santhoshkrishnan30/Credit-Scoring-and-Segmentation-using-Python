# Credit-Scoring-and-Segmentation-using-Python

# Credit Scoring and Segmentation: Overview


The process of calculating credit scores and segmenting customers based on their credit scores involves several steps. Firstly, relevant data about borrowers, such as payment history, credit utilization, credit history, and credit mix, is collected and organized. Then, using complex algorithms and statistical models, the collected data is analyzed to generate credit scores for each borrower.

These credit scores are numerical representations of the borrower’s creditworthiness and indicate the likelihood of default or timely repayment. Once the credit scores are calculated, customers are segmented into different risk categories or credit tiers based on predefined thresholds.

This segmentation helps financial institutions assess the credit risk associated with each customer and make informed decisions regarding loan approvals, interest rates, and credit limits. By categorizing customers into segments, financial institutions can better manage their lending portfolios and effectively mitigate the risk of potential defaults.

# Credit Scoring and Segmentation using Python

Now let’s get started with the task of Credit Scoring and Segmentation by importing the necessary Python libraries and the dataset:
```python
import pandas as pd
import plotly.graph_objects as go
import plotly.express as px
import plotly.io as pio
pio.templates.default = "plotly_white"

data = pd.read_csv("credit_scoring.csv")
print(data.head())
```
![image](https://github.com/santhoshkrishnan30/Credit-Scoring-and-Segmentation-using-Python/assets/145760700/25529c3f-b9b8-4db8-84e1-3bf9d3260980)

# Below is the description of all the features in the data:

1) Age: This feature represents the age of the individual.
2) Gender: This feature captures the gender of the individual.
3) Marital Status: This feature denotes the marital status of the individual.
4) Education Level: This feature represents the highest level of education attained by the individual.
5) Employment Status: This feature indicates the current employment status of the individual.
6) Credit Utilization Ratio: This feature reflects the ratio of credit used by the individual compared to their total available credit limit.
7) Payment History: It represents the monthly net payment behaviour of each customer, taking into account factors such as on-time payments, late payments, missed payments, and defaults.
8) Number of Credit Accounts: It represents the count of active credit accounts the person holds.
9) Loan Amount: It indicates the monetary value of the loan.
10) Interest Rate: This feature represents the interest rate associated with the loan.
11) Loan Term: This feature denotes the duration or term of the loan.
12)Type of Loan: It includes categories like “Personal Loan,” “Auto Loan,” or potentially other types of loans.


Now let’s have a look at column insights before moving forward:

```python
print(data.info())
```
![image](https://github.com/santhoshkrishnan30/Credit-Scoring-and-Segmentation-using-Python/assets/145760700/bfa43375-b0fc-43fa-be36-b248bc6f87f1)

Now let’s have a look at the descriptive statistics of the data:

```python
print(data.describe())
```
![image](https://github.com/santhoshkrishnan30/Credit-Scoring-and-Segmentation-using-Python/assets/145760700/10b3331d-5afb-43d2-b3a6-0d13d8ade625)

Now let’s have a look at the distribution of the credit utilization ratio in the data:
```python
credit_utilization_fig = px.box(data, y='Credit Utilization Ratio',
                                title='Credit Utilization Ratio Distribution')
credit_utilization_fig.show()
```
![image](https://github.com/santhoshkrishnan30/Credit-Scoring-and-Segmentation-using-Python/assets/145760700/65bd9e34-6164-4b49-babf-75dab39348aa)

Now let’s have a look at the distribution of the loan amount in the data:

```python
loan_amount_fig = px.histogram(data, x='Loan Amount', 
                               nbins=20, 
                               title='Loan Amount Distribution')
loan_amount_fig.show()
```
![image](https://github.com/santhoshkrishnan30/Credit-Scoring-and-Segmentation-using-Python/assets/145760700/9b21a6f2-a738-4c7f-bfdf-0e337cbea87b)

Now let’s have a look at the correlation in the data:

```python
numeric_df = data[['Credit Utilization Ratio', 
                   'Payment History', 
                   'Number of Credit Accounts', 
                   'Loan Amount', 'Interest Rate', 
                   'Loan Term']]
correlation_fig = px.imshow(numeric_df.corr(), 
                            title='Correlation Heatmap')
correlation_fig.show()
```
![image](https://github.com/santhoshkrishnan30/Credit-Scoring-and-Segmentation-using-Python/assets/145760700/21481b77-71a4-435d-aaf9-18fa47b90650)

# Calculating Credit Scores
The dataset doesn’t have any feature representing the credit scores of individuals. To calculate the credit scores, we need to use an appropriate technique. There are several widely used techniques for calculating credit scores, each with its own calculation process. One example is the FICO score, a commonly used credit scoring model in the industry.

Below is how we can implement the FICO score method to calculate credit scores:

```python
# Define the mapping for categorical features
education_level_mapping = {'High School': 1, 'Bachelor': 2, 'Master': 3, 'PhD': 4}
employment_status_mapping = {'Unemployed': 0, 'Employed': 1, 'Self-Employed': 2}

# Apply mapping to categorical features
data['Education Level'] = data['Education Level'].map(education_level_mapping)
data['Employment Status'] = data['Employment Status'].map(employment_status_mapping)

# Calculate credit scores using the complete FICO formula
credit_scores = []

for index, row in data.iterrows():
    payment_history = row['Payment History']
    credit_utilization_ratio = row['Credit Utilization Ratio']
    number_of_credit_accounts = row['Number of Credit Accounts']
    education_level = row['Education Level']
    employment_status = row['Employment Status']

    # Apply the FICO formula to calculate the credit score
    credit_score = (payment_history * 0.35) + (credit_utilization_ratio * 0.30) + (number_of_credit_accounts * 0.15) + (education_level * 0.10) + (employment_status * 0.10)
    credit_scores.append(credit_score)

# Add the credit scores as a new column to the DataFrame
data['Credit Score'] = credit_scores

print(data.head())
```
![image](https://github.com/santhoshkrishnan30/Credit-Scoring-and-Segmentation-using-Python/assets/145760700/ad21b323-f296-4609-8347-7a3e07c2c38e)

Below is how the above code works:

1) Firstly, it defines mappings for two categorical features: “Education Level” and “Employment Status”. The “Education Level” mapping assigns numerical values to different levels of education, such as “High School” being mapped to 1, “Bachelor” to 2, “Master” to 3, and “PhD” to 4. The “Employment Status” mapping assigns numerical values to different employment statuses, such as “Unemployed” being mapped to 0, “Employed” to 1, and “Self-Employed” to 2.
2) Next, the code applies the defined mappings to the corresponding columns in the DataFrame. It transforms the values of the “Education Level” and “Employment Status” columns from their original categorical form to the mapped numerical representations.
3) After that, the code initiates an iteration over each row of the DataFrame to calculate the credit scores for each individual. It retrieves the values of relevant features, such as “Payment History”, “Credit Utilization Ratio”, “Number of Credit Accounts”, “Education Level”, and “Employment Status”, from each row.

Within the iteration, the FICO formula is applied to calculate the credit score for each individual. The formula incorporates the weighted values of the features mentioned earlier: 

1) 35% weight for “Payment History”, 
2) 30% weight for “Credit Utilization Ratio”, 
3) 15% weight for “Number of Credit Accounts”, 
4) 10% weight for “Education Level”, and
5) 10% weight for “Employment Status”.
   
The calculated credit score is then stored in a list called “credit_scores”.


# Segmentation Based on Credit Scores

Now, let’s use the KMeans clustering algorithm to segment customers based on their credit scores:

```python
from sklearn.cluster import KMeans

X = data[['Credit Score']]
kmeans = KMeans(n_clusters=4, n_init=10, random_state=42)
kmeans.fit(X)
data['Segment'] = kmeans.labels_
```
Now let’s have a look at the segments:

```python
# Convert the 'Segment' column to category data type
data['Segment'] = data['Segment'].astype('category')

# Visualize the segments using Plotly
fig = px.scatter(data, x=data.index, y='Credit Score', color='Segment',
                 color_discrete_sequence=['green', 'blue', 'yellow', 'red'])
fig.update_layout(
    xaxis_title='Customer Index',
    yaxis_title='Credit Score',
    title='Customer Segmentation based on Credit Scores'
)
fig.show()
```
![image](https://github.com/santhoshkrishnan30/Credit-Scoring-and-Segmentation-using-Python/assets/145760700/313fc46f-bc07-47c1-a04e-25d01a9a18a5)

Now let’s name the segments based on the above clusters and have a look at the segments again:

```python
data['Segment'] = data['Segment'].map({2: 'Very Low', 
                                       0: 'Low',
                                       1: 'Good',
                                       3: "Excellent"})

# Convert the 'Segment' column to category data type
data['Segment'] = data['Segment'].astype('category')

# Visualize the segments using Plotly
fig = px.scatter(data, x=data.index, y='Credit Score', color='Segment',
                 color_discrete_sequence=['green', 'blue', 'yellow', 'red'])
fig.update_layout(
    xaxis_title='Customer Index',
    yaxis_title='Credit Score',
    title='Customer Segmentation based on Credit Scores'
)
fig.show()
```
![image](https://github.com/santhoshkrishnan30/Credit-Scoring-and-Segmentation-using-Python/assets/145760700/29f2e8c6-016b-4b19-b50e-a3818fbcfde3)

So this is how you can perform credit scoring and segmentation using Python.

# Summary

Credit scoring and segmentation refer to the process of evaluating the creditworthiness of individuals or businesses and dividing them into distinct groups based on their credit profiles. It aims to assess the likelihood of borrowers repaying their debts and helps financial institutions make informed decisions regarding lending and managing credit risk. 


   





