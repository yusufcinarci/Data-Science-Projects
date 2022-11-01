# Hollywood-Theatrical-Market-Synopsis-1995-to-2021

In this project, the data of hollywood film production companies from 1995 to 2021 were examined. Significant tables and graphs were created using data visualization algorithms, with the tickets sold divided into categories.

First, we will download the libraries we will use.

### import
```Python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.graph_objects as go
from plotly.subplots import make_subplots
import plotly.express as px
```

### After importing our libraries, we defined our dataset.

Annual Ticket Sales analysis
https://www.kaggle.com/datasets/johnharshith/hollywood-theatrical-market-synopsis-1995-to-2021

### We show the first five data.

```Python 
AnnualTicketSales.head()
```
![image](https://user-images.githubusercontent.com/63750425/180749789-66924887-d2c2-4b9e-9e2f-f96b26c92bb2.png)

### Average Ticket Price change yearly

```Python 
figure=plt.Figure()
plt.scatter(AnnualTicketSales['YEAR'], AnnualTicketSales['AVERAGE TICKET PRICE'])
plt.ylabel('AVERAGE TICKET PRICE')
plt.xlabel('YEAR')
plt.title('AVERAGE TICKET PRICE PER YEAR')
plt.show()
```
![image](https://user-images.githubusercontent.com/63750425/180751079-9326a453-6d4c-4d53-bce6-a21efde3ae15.png)

### Total tickets sold every year
```Python
plt.bar(AnnualTicketSales['YEAR'], AnnualTicketSales['TICKETS SOLD'])
plt.xlabel("YEAR")
plt.ylabel("TICKETS SOLD (in billions)")
plt.title("TICKETS SOLD IN BILLIONS EVERY YEAR")
plt.show()
```
![image](https://user-images.githubusercontent.com/63750425/180751238-88facd44-08f7-46cd-95d0-ba9e5d8f11bc.png)

### Correlation matrix
```Python
cormat=AnnualTicketSales.corr()
round(cormat,2)
```

![image](https://user-images.githubusercontent.com/63750425/180754581-0b24f87f-7160-4fda-8d7f-04245ec6c83f.png)

### plotting the correlation matrix 
```Python
sns.heatmap(cormat)
```

![image](https://user-images.githubusercontent.com/63750425/180754710-ede07a73-8c0f-43ab-aef7-860b38b1510e.png)

### plotting AVERAGE TICKET PRICE AND other variables in subplots to observe the correlation
```Python
plt_1=plt.figure(figsize=[15,5])
#plot 1
plt.subplot(1,3,1)
plt.scatter(y=AnnualTicketSales['AVERAGE TICKET PRICE'], x=AnnualTicketSales['TICKETS SOLD'])
plt.ylabel('AVERAGE TICKET PRICE')
plt.xlabel('TICKETS SOLD')

#plot 2
plt.subplot(1,3,2)
plt.scatter(y=AnnualTicketSales['AVERAGE TICKET PRICE'], x=AnnualTicketSales['TOTAL BOX OFFICE'])
plt.ylabel('AVERAGE TICKET PRICE')
plt.xlabel('TOTAL BOX OFFICE')

#plot 3
plt.subplot(1,3,3)
plt.scatter(y=AnnualTicketSales['AVERAGE TICKET PRICE'], x=AnnualTicketSales['TOTAL INFLATION ADJUSTED BOX OFFICE'])
plt.ylabel('AVERAGE TICKET PRICE')
plt.xlabel('TOTAL INFLATION ADJUSTED BOX OFFICE')
```
![image](https://user-images.githubusercontent.com/63750425/180754841-4783df0c-0e43-4ec5-9ed0-068a02dee670.png)

## Analyze for Genre

### TICKETS SOLD VS GENRE

### colors
```Python
colors = ['#ff9999','#66b3ff','#99ff99']

plt.pie(group_by_genre['TICKETS SOLD'], colors = colors, labels=group_by_genre['GENRE'], autopct='%1.1f%%', startangle=90, pctdistance=0.85)
#draw circle
centre_circle = plt.Circle((0,0),0.70,fc='white')
fig = plt.gcf()
fig.gca().add_artist(centre_circle)
# Equal aspect ratio ensures that pie is drawn as a circle
# ax1.axis('equal')  
plt.tight_layout()
plt.title('TICKETS SOLD VS GENRE')
plt.show()
```
![image](https://user-images.githubusercontent.com/63750425/180755178-1c9abc06-dd08-4eaf-897d-79a636c96681.png)
```Python
HighestGrossers
```
![image](https://user-images.githubusercontent.com/63750425/180755342-bd334060-8aa5-4c8f-9d80-cc83dbfb1b0e.png)

### Top 10 and Least 10 movies based on Tickets Sold
```Python
top_10_movies = HighestGrossers.nlargest(n=10, columns=['TICKETS SOLD'])
top_10_movies
```
![image](https://user-images.githubusercontent.com/63750425/180755457-876ca85b-8adc-4b02-9c27-78f4d07f7620.png)

## bar plot 
```Python
fig, ax = plt.subplots()
ax.barh(top_10_movies['MOVIE'], top_10_movies['TICKETS SOLD'], align='center')
ax.invert_yaxis()  # labels read top-to-bottom
ax.set_xlabel('TICKET SOLD')
ax.set_title('NUMBER OF TICKETS SOLD (IN 10 MILLIONS) FOR TOP 10 MOVIES')
```

![image](https://user-images.githubusercontent.com/63750425/180755560-ff9aa1d6-eb42-4219-a6e2-bef2e8a0d613.png)

## Bar plot creative types and movies in each type
## Total tickets sold every year
```Python
figure=plt.figure(figsize=(20,7))
plt.bar(PopularCreativeTypes['CREATIVE TYPES'], PopularCreativeTypes['MOVIES'])
plt.xlabel("CREATIVE TYPES")
plt.ylabel("MOVIES")
plt.title("NUMBER OF MOVIES IN EACH CREATIVE TYPE")
plt.show()
plt.show()
```
![image](https://user-images.githubusercontent.com/63750425/180755664-0c02516a-d1af-4bc5-a7f3-f3a525211135.png)

## Pie chart of Creative types and Average Gross
```Python
fig = plt.figure(figsize=[10,10])
ax = fig.add_axes([0,0,1,1])
ax.axis('equal')
ax.pie(PopularCreativeTypes['AVERAGE GROSS'], labels = PopularCreativeTypes['CREATIVE TYPES'],autopct='%1.2f%%')
plt.show()
```

![image](https://user-images.githubusercontent.com/63750425/180755841-1dcd028b-ce41-40fe-902e-d7bd09578258.png)

# Top Distributors
```Python
TopDistributors
```

![image](https://user-images.githubusercontent.com/63750425/180756022-dc65fb5e-557b-4564-bea9-9c6584c2ac4b.png)

### Distributors vs Number of movies they released
```Python
fig=plt.figure(figsize=(5,10))
ax = sns.catplot(y='MOVIES', x='DISTRIBUTORS',kind='bar', data=TopDistributors, height=6, aspect=3)
plt.ylabel('MOVIES')
plt.xlabel('DISTRIBUTORS')
```

![image](https://user-images.githubusercontent.com/63750425/180756110-b346185d-32f5-4b67-a965-5fc04e99313a.png)

## Wide Releases Count Analysis
```Python
WideReleasesCount
```
![image](https://user-images.githubusercontent.com/63750425/180756556-8940a6be-e7c1-4e78-ae76-140214d3c492.png)

### Let see the trendline of total movies released by 6 major production from 1995 to 2021
```Python
fig=plt.figure(figsize=(5,10))
ax = sns.catplot(y='TOTAL MAJOR 6', x='YEAR',kind='bar', data=WideReleasesCount, height=6, aspect=3)
plt.ylabel('TOTAL NUMBER OF MOVIES')
plt.xlabel('YEARS')
plt.title('TOTAL NUMBER OF MOVIES RELEASED BY 6 MAJOR PRODUCTION FROM 1995 RO 2021')
```
![image](https://user-images.githubusercontent.com/63750425/180756738-a14b889d-2ba3-4b1a-a40f-574600fabe40.png)

In our project above, we tried to show some important and striking graphics. You can find all the files of our project on our data science journey in the github repository.
