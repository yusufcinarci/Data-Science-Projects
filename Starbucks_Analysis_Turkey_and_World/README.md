# Starbucks Analysis Turkey and World

In this project, firstly the brands for coffee in the world and then these brands in Turkey were examined. The data from the dataset, which you can find in the repo, was first organized using data cleaning algorithms. These cleaned data were then graphically extracted using data visualization algorithms.


# Importing Libraries
```Python
import numpy as np 
import pandas as pd
import seaborn as sns
from matplotlib import pyplot as plt
import warnings
from mpl_toolkits.basemap import Basemap
%matplotlib inline
sns.set(style="white", context="talk")
warnings.simplefilter(action='ignore', category=FutureWarning)
warnings.filterwarnings("ignore",category=plt.cbook.mplDeprecation)

```

# Read Dataset(head)

```Python
data.head()
```
![1](https://user-images.githubusercontent.com/77057546/186663466-37eabd6f-0999-497a-89c9-549aa87cf7d6.png)

# How many brands are available in the data

## Distribution of Brands
```Python
x=data['Brand'].value_counts()
print(x)
plt.figure(figsize=(8, 6))
fig=sns.barplot(x.index,x.values)
plt.xlabel('Brands')
plt.ylabel('Density')
fig.set_title('Distribution of Brands')
```

![2](https://user-images.githubusercontent.com/77057546/186663776-6e1a8627-5c4c-4651-a1b0-82aad0af7e0b.png)

### Which country does 'Starbucks' brand use?

```Python
Starbucks=data[data['Brand']=='Starbucks']['Country'].value_counts()[1:10]
print(Starbucks)
plt.figure(figsize=(8, 6))
fig=sns.barplot(Starbucks.index,Starbucks.values)
plt.xlabel('Country')
plt.ylabel('Density')
fig.set_title('Distribution of Starbucks Brand')
```

![4](https://user-images.githubusercontent.com/77057546/186664406-12afe0ea-2423-4f49-98f9-8697868aec84.png)


# How many cities are Starbucks stores available?


```Python
City=data['City'].value_counts()
print(City)
print('Total number of cities is',len(City))
plt.figure(figsize=(12,8))
fig=sns.barplot(City.index[:10],City.values[:10])
plt.xlabel('City')
plt.ylabel('Density')
fig.set_title('Distribution of City')
```

![5](https://user-images.githubusercontent.com/77057546/186665247-84b8e788-8424-4588-8ed1-72e144d9c986.png)

## How many state or province are Starbucks stores available?


```Python
State=data['State/Province'].value_counts()
print(State)
print('Total number of states is',len(State))
plt.figure(figsize=(12,8))
fig=sns.barplot(State.index[:10],State.values[:10])
plt.xlabel('State')
plt.ylabel('Density')
fig.set_title('Distribution of State')
```

![6](https://user-images.githubusercontent.com/77057546/186665523-9649147b-9792-4f64-8d73-3af58c0288bb.png)

## Timezone

```Python
Timezone=data['Timezone'].value_counts()
print(Timezone)
print('Total number of timezone is',len(Timezone))
plt.figure(figsize=(25,8))
fig=sns.barplot(Timezone.index[:5],Timezone.values[:5])
plt.xlabel('State')
plt.ylabel('Density')
fig.set_title('Top 10 Country Distribution of Timezone')
```
![7](https://user-images.githubusercontent.com/77057546/186665842-e02844bb-cbe3-4327-8689-45854a825d29.png)

# World Map of Starbucks Store

```Python
plt.figure(figsize=(16,16))
worldmap = Basemap(projection='mill', 
              llcrnrlat=-80,
              urcrnrlat=80,
              llcrnrlon=-180,  
              urcrnrlon=180, 
              resolution='l')
worldmap.drawcoastlines()
worldmap.drawcountries()
worldmap.drawmapboundary(fill_color='white')

# Load in Longitude and Latitude data
Longitude = data["Longitude"].astype(float)
Latitude = data["Latitude"].astype(float)
x, y = worldmap(list(Longitude), list(Latitude))
worldmap.plot(x, y,'bo',markersize =5,color="green" )
plt.title('The World Map of Starbucks Store')
plt.show()
```

![8](https://user-images.githubusercontent.com/77057546/186666123-6e5d8d53-277f-4e5f-91ab-53a7292aac50.png)


# Specific Approximation for Turkey Country

```Python
TurkeyData=data[data['Country']=='TR']
TurkeyData.head()
```

![9](https://user-images.githubusercontent.com/77057546/186666376-8df73c49-af29-4536-a14a-c2b57c2cd0ed.png)

# Distribution of City


```Python
TurkeyCity=TurkeyData['City'].value_counts()
print('Total number of city is', len(TurkeyCity))
plt.figure(figsize=(14,9))
fig=sns.barplot(TurkeyCity.index[:10],TurkeyCity.values[:10])
plt.xlabel('Cities')
plt.ylabel('Density')
fig.set_title('Distribution of City')
```

![10](https://user-images.githubusercontent.com/77057546/186666663-6dbece8c-5402-4969-b31e-7b5e9916ff8f.png)


# Turkey Starbucks Map

```Python
plt.figure(figsize=(16,16))
map = Basemap(projection='stere', 
              lat_0=38, lon_0=37,
              llcrnrlon=25, 
              llcrnrlat=34, 
              urcrnrlon=50, 
              urcrnrlat=43,resolution='l',area_thresh=10000,rsphere=6371200.)
map.drawcoastlines()
map.drawcountries()
map.drawmapboundary(fill_color='white')

# Load in Longitude and Latitude data
Longitude = TurkeyData["Longitude"].astype(float)
Latitude = TurkeyData["Latitude"].astype(float)
x, y = map(list(Longitude), list(Latitude))
map.plot(x, y,'bo',markersize =7,color="red" )
plt.title('The Turkey Map of Starbucks Store')
plt.show()
```

![11](https://user-images.githubusercontent.com/77057546/186667330-62f76b9c-e072-47a6-bda0-20039ad40464.png)

See you on another project.

## MAY THE FORCE BE WITH YOU!!!
