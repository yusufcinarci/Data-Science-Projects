We developed a song recommendation system for the user with the data we received from our Spotify song dataset. Data set and other applications are given in the description. Have a nice day.

### import
```Python
import pandas as pd
import numpy as np
import seaborn as sns
import plotly.express as px
import plotly.graph_objects as go
import matplotlib.pyplot as plt
import plotly
from sklearn.preprocessing import StandardScaler
from scipy.spatial import distance
import copy
import warnings

warnings.filterwarnings("ignore")
plotly.offline.init_notebook_mode (connected = True)
```
## Having First Look At The Data
```Python
data.head()
```
![image](https://user-images.githubusercontent.com/63750425/182090399-8e159e3d-3d1e-43ff-8245-c5b15ee6d7ab.png)

```Python
cols=list(data.columns[11:])
del cols[7]
df=copy.deepcopy(data)
df.drop(columns=cols,inplace=True)
sns.set(style="ticks", context="talk")
plt.style.use("dark_background")
sns.pairplot(df,corner=True,hue='genre')
```
![image](https://user-images.githubusercontent.com/63750425/182090600-c2f2a269-a620-4713-9e73-24de868673d1.png)

```Python
px.box(data_frame=data,y='duration_ms',color='genre')
```

![image](https://user-images.githubusercontent.com/63750425/182090680-c7bfbc15-c9e6-452b-b1d1-238a0a8b99e1.png)

```Python
x=list(data.corr().columns)
y=list(data.corr().index)
values=np.array(data.corr().values)
fig = go.Figure(data=go.Heatmap(
    z=values,
    x=x,
    y=y,
                   
    
                   hoverongaps = False))
fig.show()
```

![image](https://user-images.githubusercontent.com/63750425/182090778-fa40ff7b-3a02-4ad1-9285-1567701f17c1.png)

## Let's Make A Recommendation System

```Python
data=data.dropna(subset=['song_name'])
```

## Creating a new dataframe with required features




```Python
# Making a weight matrix using euclidean distance
def make_matrix(data,song,number):
    df=pd.DataFrame()
    data.drop_duplicates(inplace=True)
    songs=data['song_name'].values
#    best = difflib.get_close_matches(song,songs,1)[0]
    best=find_word(song,songs)
    print('The song closest to your search is :',best)
    genre=data[data['song_name']==best]['genre'].values[0]
    df=data[data['genre']==genre]
    x=df[df['song_name']==best].drop(columns=['genre','song_name']).values
    if len(x)>1:
        x=x[1]
    song_names=df['song_name'].values
    df.drop(columns=['genre','song_name'],inplace=True)
    df=df.fillna(df.mean())
    p=[]
    count=0
    for i in df.values:
        p.append([distance.euclidean(x,i),count])
        count+=1
    p.sort()
    for i in range(1,number+1):
        print(song_names[p[i][1]])
 ```
 
 ```Python
a=input('Please enter The name of the song :')
b=int(input('Please enter the number of recommendations you want: '))
make_matrix(df,a,b)
 ```
 
 ![image](https://user-images.githubusercontent.com/63750425/182091192-e953dbb6-f896-4d93-bdf9-1e7ad7176115.png)
 ```Python
def make_matrix_correlation(data,song,number):
    df=pd.DataFrame()
    data.drop_duplicates(inplace=True)
    songs=data['song_name'].values
#    best = difflib.get_close_matches(song,songs,1)[0]
    best=find_word(song,songs)
    print('The song closest to your search is :',best)
    genre=data[data['song_name']==best]['genre'].values[0]
    df=data[data['genre']==genre]
    x=df[df['song_name']==best].drop(columns=['genre','song_name']).values
    if len(x)>1:
        x=x[1]
    song_names=df['song_name'].values
    df.drop(columns=['genre','song_name'],inplace=True)
    df=df.fillna(df.mean())
    p=[]
    count=0
    for i in df.values:
        p.append([distance.correlation(x,i),count])
        count+=1
    p.sort()
    for i in range(1,number+1):
        print(song_names[p[i][1]])
  ```
 ```Python
e=input('Please enter The name of the song :')
f=int(input('Please enter the number of recommendations you want: '))
make_matrix_correlation(df,e,f)
  ```
  
  ![image](https://user-images.githubusercontent.com/63750425/182091405-fb736d12-bf27-439a-a0a1-5b4276539b08.png)

## See you on other data science projects. Take care of yourselves:)
