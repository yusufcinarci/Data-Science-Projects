Hello, today I will tell you the details of the stock market analysis project with python.

# Stock Market Analysis with Pandas Python

First, let's start our project by importing the libraries. 
# import

``` 
import pandas_datareader.data as web 
import datetime
import matplotlib.pyplot as plt
import numpy as np
%matplotlib inline
``` 
After defining our libraries, we determine our start and end dates for our stock market analysis.

``` 
start = datetime.datetime(2021,1,1)
end = datetime.datetime(2022,2,25)
``` 

## What is Pandas Datareader in Python?
Pandas Datareader is a Python package that allows us to create a pandas DataFrame by using some popular data sources available on the internet including:

1.Yahoo Finance
2.Google Finance
3.Morningstar
4.IEX
5.Robinhood
6.Engima
7.Quandl
8.FRED
9.World Bank
10.OECD and many more.

Yes, we learned what datareader is, now let's use datareader to pull google stock market data from yahoo source.

``` 
google = web.DataReader("GOOGL", "yahoo", start, end)
``` 
``` 
google.head()
``` 
![155878863-6bb6dfab-5d67-4e74-bfde-7ae57b740942](https://user-images.githubusercontent.com/77057546/160421324-426e39bd-253b-4c80-a4ec-c046c65ac710.png)


``` 
google.tail()
``` 
![tail](https://user-images.githubusercontent.com/77057546/160421687-3515390f-7ec9-406b-a4cf-c93ec9c7ca27.png)

Yes, we have extracted the change table of the google stock market data within the first and last 5 days in the date ranges we defined above.
``` 
google['Open'].plot(label = 'GOOGL Open price', figsize= (16,8))
google['Close'].plot(label = 'GOOGL Close price')
google['High'].plot(label = 'GOOGL High price')
google['Low'].plot(label = 'GOOGL Low price')
plt.legend()
plt.title('Google Stock Prices')
plt.ylabel('Stock Price')
plt.show()
``` 
Thanks to the code blocks above, we showed Google's data such as opening, closing, volume in certain time intervals in the stock market on the plot chart.

Now three big automobile and engine companies; Let's examine the stock market data of TESLA, FORD AND GENERAL MOTORS.
``` 
tesla = web.DataReader("TSLA",'yahoo', start, end)
ford  = web.DataReader("F", 'yahoo', start, end)
gm = web.DataReader("GM", 'yahoo', start,end)
``` 
Just like we got Google data, we got the data of three companies from yahoo using pandas datareader.
``` 
tesla.to_csv('Tesla_Stock.csv')
ford.to_csv('Ford_Stock.csv')
gm.to_csv('GM_Stock.csv')
``` 
We will use the following lines of code to convert the captured data into a csv file.
```
tesla.head()
```
![asdadad](https://user-images.githubusercontent.com/77057546/160422583-19b6984b-a9e7-4a7c-9e32-bbd7eeeea0b8.png)
```
tesla['Open'].plot(label = 'Tesla', figsize= (16,8))
gm['Open'].plot(label = 'GM')
ford['Open'].plot(label = 'Ford')
plt.legend()
plt.title('Stock Prices of Tesla, GM, Ford')
plt.ylabel('Stock Price')
plt.show()
```
Thanks to the code blocks we wrote above, you can see our chart below, which compares the opening data of three companies.

![2](https://user-images.githubusercontent.com/77057546/160422947-fa6b5105-c1f2-4030-9f2c-edd268f5065b.png)
```
tesla['Total Traded']= tesla['Open'] * tesla['Volume']
gm['Total Traded']= gm['Open'] * gm['Volume']
ford['Total Traded']= ford['Open'] * ford['Volume']
```

```
tesla['Total Traded'].plot(label = 'Tesla', figsize= (16,8))
gm['Total Traded'].plot(label = 'GM')
ford['Total Traded'].plot(label = 'Ford')
plt.legend()
plt.ylabel('Total Traded')
```
![3](https://user-images.githubusercontent.com/77057546/160423314-58d0af42-8e2f-4010-bc60-dce7ec722c6c.png)

Thanks to the lines of code we wrote above, we compared the total trade data of 3 companies.

## Moving average

In statistics, a moving average (rolling average or running average) is a calculation to analyze data points by creating a series of averages of different subsets of the full data set. It is also called a moving mean (MM)[1] or rolling mean and is a type of finite impulse response filter. Variations include: simple, cumulative, or weighted forms. Given a series of numbers and a fixed subset size, the first element of the moving average is obtained by taking the average of the initial fixed subset of the number series. Then the subset is modified by "shifting forward"; that is, excluding the first number of the series and including the next value in the subset.

```
tesla['Open'].plot(label = 'No Moving Average', figsize= (16,8))
tesla['MA50']=tesla['Open'].rolling(50).mean()
tesla['MA50'].plot(label='MA50')
tesla['MA200']= tesla['Open'].rolling(200).mean()
tesla['MA200'].plot(label='MA200')
plt.legend()
```
![4](https://user-images.githubusercontent.com/77057546/160423757-8af906a8-182d-4b9c-a5ea-8c8f5516f268.png)

In the chart we have seen above, we have drawn the moving average chart of the tesla company.

## What is a Scatter Matrix?

A scatter matrix (pairs plot) compactly plots all the numeric variables we have in a dataset against each other one. In Python, this data visualization technique can be carried out with many libraries but if we are using Pandas to load the data, we can use the base scatter_matrix method to visualize the dataset. Now let's create a scatter matrix together and display our data on the scatter map.
```
from pandas.plotting import scatter_matrix
import pandas as pd
```
```
car_comp = pd.concat([tesla['Open'],gm['Open'],ford['Open']],axis = 1)
car_comp.columns = ['Tesla Open', 'GM Open', 'Ford Open']
```

```
scatter_matrix(car_comp,figsize = (12,12))
```
![5](https://user-images.githubusercontent.com/77057546/160424235-8e85b471-cf28-455d-a103-b1d2f19e7c40.png)

Yes, in this project I tried to show you how to make a simple stock market data analysis application over python.

I wish you a healthy day.






