# Turkey Tourist Analysis with Python

In this project, the number of tourists coming to Turkey between 2008-2021 was analyzed. The data from the data set you can find in the warehouse was first organized using data cleaning algorithms. These cleaned data were then output graphically using data visualization algorithms.


### import
```Python
import numpy as np 
import pandas as pd 

from matplotlib.pyplot import figure
import matplotlib.pyplot as plt
import matplotlib.dates as mdates
import seaborn as sns

```

# Read Dataset(head)

```Python
data.head()
```
![1](https://user-images.githubusercontent.com/77057546/186473041-d3f0a96b-06ed-49d1-ae62-c289d56c6fd9.png)

# Data Query and Visualization

## Top 3 Continents Total Number Where Tourists Originated


```Python
cvalues = continents[c_final.index]
cvalues.head()
```

![2](https://user-images.githubusercontent.com/77057546/186473647-4dc3b3b8-35d0-4b5b-b738-5aa6d89c127b.png)

```Python

plt.style.use('ggplot')
plt1=figure(figsize=(10, 5), dpi=120)
plt.xticks(fontsize=17, rotation=45)
plt.yticks(fontsize=17)

plt.plot(continents['DATE'],cvalues)
plt.xlabel("Date")
plt.ylabel("Total number of Tourists")
plt.title('Continents Comparison ')
plt.legend(cvalues)
plt.show

```
![3](https://user-images.githubusercontent.com/77057546/186476276-bbdcd5ba-7b8b-461e-ab9e-77b46fc06c6c.png)

## Bottom 30 Countries after Calculation

What we see in the graph are the 30 countries that experienced the most decline in tourist numbers after 2015 compared to before 2015. 66% fewer tourists came from Japan and 63% less from Italy, and when we look at the rest of the list, it is obvious that predominantly European and 1st World countries prefer Turkey less.

```Python
figure(figsize=(16, 8), dpi=120)
plt.barh(fsmall.index,fsmall)
plt.title('Bottom 30 Countries after Calculation ')
plt.ylabel('Country name')
plt.xlabel('Percentage of Tourist Losses')
plt.show()
```

![4](https://user-images.githubusercontent.com/77057546/186476733-55c306dc-8bdd-417f-a700-fa8c949889e1.png)

## Top 30 Countries after Calculation

What we see in this graph are the 30 countries that experienced the most growth in total tourist numbers after 2015 compared to before 2015. After 2015, the number of tourists increased by almost 400% from Qatar and by almost 300% from Bahrain. When we look at the rest of the list, we cannot see any developed European country or 1st World country.

```Python
figure(figsize=(16, 8), dpi=120)
plt.barh(flarge.index,flarge, color='blue')
plt.title('Top 30 Countries after Calculation')
plt.ylabel('Country name')
plt.xlabel('Percentage of Tourist Gain')
plt.show()
```

![5](https://user-images.githubusercontent.com/77057546/186477105-e7007bc4-0026-4b43-9e26-9eacf2436fa1.png)

# Total Number of Tourist by Year

Except for 2015 and 2019, we can see that the number of tourists has increased. We see that in 2019 before the pandemic, total number of tourists reached almost 45 million people.

```Python
figure(figsize=(10, 5), dpi=120)
plt.xticks(finalcountry.index,fontsize=17, rotation=45)
plt.plot(finalcountry.index,finalcountry['SUM OF YEAR'],'b', linewidth=1)
plt.xlabel("Year")
plt.ylabel("Total Number of Tourists")
plt.title('Total Number of Tourists by Year ')
plt.legend()
plt.show
```
![6](https://user-images.githubusercontent.com/77057546/186477847-6cbb2fc0-503c-4485-8240-d5488967551e.png)

## Total Revenue by Year

```Python
figure(figsize=(10, 5), dpi=120)
plt.xticks(revdata['Yr'],fontsize=17, rotation=45)
plt.plot(revdata['Yr'],revdata['Total Revenue'], linewidth=1)
plt.xlabel("Date")
plt.ylabel("Total Revenue")
plt.title('Total Revenue by Year')
plt.legend()
plt.show
```

![7](https://user-images.githubusercontent.com/77057546/186478186-ec81ae45-6b2f-4de5-ab04-7af7793dd91d.png)

## Total Spending($) per Person by Year

```Python
fig_hist = px.histogram(data_frame=data, x='Olus zamani')
figure(figsize=(10, 5), dpi=120)
plt.xticks(revdata.index,fontsize=17, rotation=45)
plt.plot(revdata.index,revdata['Spending1Person'], 'm', linewidth=1)
plt.xlabel("Date")
plt.ylabel("Total Amount of US Dollars")
plt.title('Total Spending($) per Person by Year ')
plt.legend()
plt.show
```

![8](https://user-images.githubusercontent.com/77057546/186478567-3b32cf37-e651-4215-ba64-2607a9de3ec1.png)


```Python
fig, ax = plt.subplots(4,sharex='col', figsize=(20,10)) 
plt.xticks(revdata.index,fontsize=17, rotation=45)
ax[0].plot(revdata.index,revdata['Total Revenue'], 'r', linewidth=3)
ax[0].legend(['Total Revenue'])
ax[1].plot(revdata.index,revdata['Total People'], 'b',linewidth=3)
ax[1].legend(['Total People'])
ax[2].plot(revdata.index,revdata['Spending1Person'], 'm',linewidth=3)
ax[2].legend(['Spending per Person ($)'])
ax[3].plot(revdata.index,revdata['ValueofTL'], 'g',linewidth=3)
ax[3].legend(['Value of Turkish Lira'])
plt.title('Most Recent Graph Overview')
```

![9](https://user-images.githubusercontent.com/77057546/186478909-fa677670-0e60-45ac-89f3-5da86bed8f2a.png)


# Correlation Test and Visualation with Heatmap

Looking this scatter plots, we would easily say between total people of tourist and total revenue has strong positive linear relationship. Let's check the correlation coefficients.

```Python
fig, ax = plt.subplots(3, 2, figsize=(20,20))
figure(figsize=(14, 7), dpi=120)

ax[0,0].scatter(revdata['Total Revenue'],revdata['Total People'])
ax[0,0].set_xlabel("Total Revenue")
ax[0,0].set_ylabel("Total People")

ax[0,1].scatter(revdata['Total People'],revdata['Spending1Person'])
ax[0,1].set_xlabel("Total People")
ax[0,1].set_ylabel("Spending per Person")
       
ax[1,0].scatter(revdata['Total Revenue'],revdata['Spending1Person'])
ax[1,0].set_xlabel("Total Revenue")
ax[1,0].set_ylabel("Spending per Person")

ax[1,1].scatter(revdata['Total People'],revdata['ValueofTL'])
ax[1,1].set_xlabel("Total People")
ax[1,1].set_ylabel("Value of Turkish Lira")
       
ax[2,0].scatter(revdata['Total Revenue'],revdata['ValueofTL'])
ax[2,0].set_xlabel("Total Revenue")
ax[2,0].set_ylabel("Value of Turkish Lira")

ax[2,1].scatter(revdata['Spending1Person'],revdata['ValueofTL'])
ax[2,1].set_xlabel("Spending per Person")
ax[2,1].set_ylabel("Value of Turkish Lira")
plt.show()
```

![11](https://user-images.githubusercontent.com/77057546/186479884-288dcb01-45d7-40cb-8ca7-3072599969c7.png)

```Python
sns.heatmap(revdatacor, cmap="Blues")
sns.set(font_scale = 2)
plt.show()
revdatacor
```

![10](https://user-images.githubusercontent.com/77057546/186480139-96ffd244-5c19-4ee4-8ab6-320f65bfe59d.png)


## Average Number of Tourists by Month

```Python
figure(figsize=(10, 5), dpi=120)
plt.xticks(fontsize=17, rotation=45)
plt.yticks(fontsize=17)
plt.title('Average Number of Tourists by Month')
plt.bar(finalmonth.index,finalmonth, color='green')
plt.show()
```

![12](https://user-images.githubusercontent.com/77057546/186480473-83325e3c-252e-47e4-99c1-5a088e329951.png)

See you on another project.

## MAY THE FORCE BE WITH YOU!!!
