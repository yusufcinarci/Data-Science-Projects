# App-Store-Data-Analysis
With this data analysis project, we examined the app_store data. Dataset links are given below.

Dataset link: https://www.kaggle.com/datasets/ramamet4/app-store-apple-data-set-10k-apps

### import
```Python

import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
import seaborn as sns
from warnings import filterwarnings
filterwarnings("ignore")
!pip3 install ppscore
import ppscore as pps
#Import Library RobustScaler
from sklearn.preprocessing import RobustScaler
#Cluster Model
from sklearn.cluster import KMeans 
from sklearn.metrics import silhouette_score
```
```Python
data.head()
```

![image](https://user-images.githubusercontent.com/63750425/187078070-6c17a887-d6f3-4b50-8bef-815a1cbbc0d0.png)

```Python
#data about data
data.info()
```

![image](https://user-images.githubusercontent.com/63750425/187078110-91115d8e-5e17-4497-b3fe-c206fc8ea471.png)

```Python
plt.style.use('fivethirtyeight')
plt.figure(figsize=(6,4))

plt.subplot(2,1,2)
plt.title('Visual price distribution')
sns.stripplot(data=paid_apps,y='price',jitter= True,orient = 'h' ,size=6)
plt.show()
```


![image](https://user-images.githubusercontent.com/63750425/187078130-036cfa71-f672-4968-823f-4db37b96ae26.png)

```Python
Top_Apps = Top_Apps.sort_values('price', ascending=False)

visualizer(Top_Apps.price,Top_Apps.track_name, "bar", "TOP 7 APPS ON THE BASIS OF PRICE","Price (in USD)","APP NAME")
#names of track in y axis to be readable
```

![image](https://user-images.githubusercontent.com/63750425/187078148-f9ecf741-bc2d-49ca-80e8-c4ab4816f9ae.png)


```Python
paid_apps.head(5)
```

![image](https://user-images.githubusercontent.com/63750425/187078169-b8619781-c9d2-4d2a-90ca-116176b5b91b.png)

```Python
data.prime_genre.value_counts()
```

![image](https://user-images.githubusercontent.com/63750425/187078195-6d42de95-b01f-4cb4-a3f2-c2d0566fd867.png)

```Python
#Top_Categories accorrding number of apps
new_data_cate.head(10)
```

![image](https://user-images.githubusercontent.com/63750425/187078214-ab797803-b1d9-4fe5-b588-12e419685947.png)

```Python
sns.barplot(y = 'prime_genre',x = '# of Apps', data=new_data_cate.head(10))
```
![image](https://user-images.githubusercontent.com/63750425/187078231-04b82727-87b4-4e4c-85cd-0bf8eab4481a.png)

```Python
plt.figure(figsize=(10,5))
plt.scatter(y=paid_apps.prime_genre ,x=paid_apps.price,c='DarkBlue')
plt.title('Price & Category')
plt.xlabel('Price')
plt.ylabel('Category')
plt.show()
```

![image](https://user-images.githubusercontent.com/63750425/187078257-8909728d-e6a5-4c73-b967-da3a7f9d9cc4.png)


### What about paid apps Vs Free apps ?

```Python
free_apps.head(3)
```

![image](https://user-images.githubusercontent.com/63750425/187078288-73f49468-f067-473c-b494-6ac78045e9bc.png)

```Python
names = ['sum_free', 'sum_paid']
values = [sum_free, sum_paid]
plt.figure(figsize=(3, 3))
plt.suptitle('Count of free and paid apps')
plt.bar(names, values)
plt.show()
```

![image](https://user-images.githubusercontent.com/63750425/187078304-b958e571-48fb-4446-9f3c-a8a8ac18fa64.png)


### of paid apps greater than # of free apps
```Python
# for pie chart
pies = fig[['%free','%paid']].head()
pies.columns=['free %','paid %']
```
```Python
plt.figure(figsize=(15,10))
pies.T.plot.pie(subplots=True,figsize=(20,4),colors=['#D62598','#FBDD7A'],autopct = '%1.0f%%')
plt.show()
```

![image](https://user-images.githubusercontent.com/63750425/187078343-41d7e3e3-c56c-4067-890b-7fce2f9465fb.png)

```Python
visualizer(paid_apps['user_rating'],paid_apps.prime_genre, "bar", "User-Rating in All Categories","User_Rating","Categories")
```

![image](https://user-images.githubusercontent.com/63750425/187078361-01d20b62-5a82-489c-9d1f-0ab921f07082.png)

```Python
Top_Apps = Top_Apps.sort_values('price', ascending=False)

visualizer(Top_Apps.user_rating,Top_Apps.track_name, "bar", "TOP 7 APPS ON THE BASIS OF PRICE With User-Rating","User_Rating","APP NAME")
#names of track in y axis to be readable
```

![image](https://user-images.githubusercontent.com/63750425/187078374-f0195f76-919e-4899-bba0-2717a299b6d2.png)

```Python
BlueOrangeWapang = ['#fc910d','#f5ed05','#09ed52','#ed3b09','#e01bda']
plt.figure(figsize=(15,15))
label_names=data.broad_genre.value_counts().sort_index().index
size = data.broad_genre.value_counts().sort_index().tolist()

my_circle=plt.Circle( (0,0), 0.5, color='white')
plt.pie(size, labels=label_names, colors=BlueOrangeWapang ,autopct = '%1.0f%%',)
p=plt.gcf()
p.gca().add_artist(my_circle)
plt.show()
```


![image](https://user-images.githubusercontent.com/63750425/187078402-bc1e8765-a9ca-4932-80a0-65cb9b177adb.png)

```Python
plt.figure(figsize=(15,15))
f=pd.DataFrame(index=np.arange(0,10,2),data=five_ca['free'].values,columns=['num'])
p=pd.DataFrame(index=np.arange(1,11,2),data=five_ca['paid'].values,columns=['num'])
final = pd.concat([f,p],names=['labels']).sort_index()
final.num.tolist()

plt.figure(figsize=(25,25))
group_names=data.broad_genre.value_counts().sort_index().index
group_size=data.broad_genre.value_counts().sort_index().tolist()
h = ['Free', 'Paid']
subgroup_names= 5*h
sub= ['#45cea2','#fdd470']
subcolors= 5*sub
subgroup_size=final.num.tolist()


# First Ring (outside)
fig, ax = plt.subplots()
ax.axis('equal')
mypie, _ = ax.pie(group_size, radius=2.5, labels=group_names, colors=BlueOrangeWapang)
plt.setp( mypie, width=1.2, edgecolor='white')

# Second Ring (Inside)
mypie2, _ = ax.pie(subgroup_size, radius=1.6, labels=subgroup_names, labeldistance=0.7, colors=subcolors)
plt.setp( mypie2, width=0.8, edgecolor='white')
plt.margins(0,0)

# show it
plt.show()
```

![image](https://user-images.githubusercontent.com/63750425/187078440-2361cd9a-0125-4cce-9b35-d86c899a07b3.png)

