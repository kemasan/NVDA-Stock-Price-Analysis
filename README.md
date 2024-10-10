# NVDA-Stock-Price-Analysis
Historical price analysis 

This project performs a technical analysis of NVIDIA (NVDA) historical stock price data. The goal is to calculate and visualize key financial metrics such as moving averages, daily returns, volatility, and support and resistance levels.

## Project Overview
This project involves the following key steps:

1. Fetching stock price data from Yahoo Finance.
2. Calculating important technical indicators: 30-day and 90-day moving averages.
3. Analyzing daily returns and rolling volatility and visualizing trends.
4. Generating a correlation heatmap of the selected features.
5. Identifying and plotting support and resistance levels.
   

## Requirements
Make sure you have the following Python libraries installed:

```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from yahoo_fin.stock_info import get_data
from scipy.signal import argrelextrema

```

## Dataset
The analysis uses a dataset of NVIDIA (NVDA) historical stock prices fetched from Yahoo Finance.

## Instructions
### Step 1: Fetch Data from Yahoo Finance
We fetch the stock price data using the yahoo_fin library. The code below retrieves data for NVIDIA (NVDA) from Yahoo Finance for a specified time period and saves it as a CSV file:

```
ticker = "NVDA" 
start_date = "2024-01-01"
end_date = "2024-10-01"
data = get_data(ticker, start_date=start_date, end_date=end_date)
data.to_csv("NVDAQ324.csv")

```
This step will create a file called NVDAQ324.csv, which will be used for analysis.

### Step 2: Load the Data
We read the stock price data from the saved CSV file:

```
df = pd.read_csv('NVDAQ324.csv')
df['date'] = pd.to_datetime(df['date'])
df.set_index('date', inplace=True)

```
### Step 3: Calculate Daily Returns and Volatility
We calculate daily percentage price changes to track the stock’s day-to-day performance and 30-day rolling volatility for risk assessment:
```
df['daily_return'] = df['close'].pct_change()
df['std_dev'] = df['daily_return'].rolling(window=30).std() * np.sqrt(30)

```

### Step 4: Calculate Moving Averages and Correlation between data
We compute the 30-day and 90-day moving averages to understand the stock’s short-term and long-term trends:

```
df['MA30'] = df['close'].rolling(window=30).mean()
df['MA90'] = df['close'].rolling(window=90).mean()
corr = df[['close', 'MA30', 'MA90', 'volume', 'daily_return']].corr()

```

### Step 5: Identify Support and Resistance Levels
Using local minima and maxima, we identify support and resistance levels:

```
n = 30
support = df.iloc[argrelextrema(df['close'].values, np.less_equal, order=n)[0]]['close']
resistance = df.iloc[argrelextrema(df['close'].values, np.greater_equal, order=n)[0]]['close']

```
