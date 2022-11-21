# Turkey Earthquake Analysis 1915-2021 with Python

In this project, earthquakes in Turkey from 1915 to 2021 were analyzed. The data taken from the data set, which you can find in the repo, was first organized using data cleaning algorithms. Afterwards, these cleaned data were printed out as graphics and animation using data visualization algorithms.

Kandilli Observatory, or more formally Kandilli Observatory and Earthquake Research Institute (KOERI, Turkish: Kandilli Rasathanesi ve Deprem Araştırma Enstitüsü) is a Turkish observatory, which is also specialized on earthquake research. It is situated in Kandilli neighborhood of Üsküdar district on the Anatolian side of Istanbul, atop a hill overlooking Bosporus.

The dataset includes the Earthquakes that occurred in Turkey and the close coverage area. This data includes only the earthquakes in the range from 1915 to January 2021 and greater than 3.5 magnitudes.

All the data here is owned by "Boğaziçi Üniversitesi Rektörlüğü" and it can only be used for uncommercial issues with regards to "Boğaziçi Üniversitesi Kandilli Rasathanesi ve Deprem Araştırma Enstitüsü Bölgesel Deprem-Tsunami İzleme ve Değerlendirme Merkezi".

### import
```Python
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns
import plotly.figure_factory as ff
import plotly.graph_objects as go
import numpy as np
import plotly.express as px
import re

import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)

import os
for dirname, _, filenames in os.walk('/kaggle/input'):
    for filename in filenames:
        print(os.path.join(dirname, filename))

```
# Read Dataset(head and tail) 

```Python
data.head()
```
![1](https://user-images.githubusercontent.com/77057546/186189847-293b6f47-36b8-4a97-b9d7-73165fbf572c.png)

```Python
data.tail()
```

![2](https://user-images.githubusercontent.com/77057546/186189855-271b444b-4ccf-4933-8dcb-611d1cb2bb41.png)

# Dropping Split-Second
According to my pre-analysis,noticed that some data point has incorrect second information. They are greater than 60 seconds.

```Python
data['Olus tarihi'][11109] + '  ' +data['Olus zamani'][11109]

# Splitting the dataframe due to before and after the '.'
data['Olus zamani'] = data['Olus zamani'].str.split('.').str[0]

# Saniye is greater than 60 for some data points. I located and dropped them
for i in range(0, len(data)):
    if float(data['Olus zamani'][i][6:]) >= 60:
        data = data.drop(i)
data = data.reset_index(drop=True)

# Creating single column for date and time
olus_zamani = pd.to_datetime(data['Olus tarihi'] + ' ' + data['Olus zamani'])
data['Olus zamani'] = olus_zamani
data.drop(['Olus tarihi'], axis=1, inplace=True)
data.head()

```
![3](https://user-images.githubusercontent.com/77057546/186190543-5a157045-3895-4ae7-9cc6-58250369d482.png)

# Data Cleaning of 'Yer' Feature

According to my pre-analysis, I noticed that most of the "Yer" feature data points have incorrect or corrupted points.

```Python
data['Yer'][4157]
```
![4](https://user-images.githubusercontent.com/77057546/186191146-9b941910-68c1-41d5-93fa-044ee6e72808.png)

```Python
# Getting rid of the string values start with '['
Yer = Yer[0].str.split('[').str[0].to_frame()
Yer.columns = ['Yer']

# Placing the created Yer column to the original dataset
data['Yer'] = Yer

# Some data points have missing letters due to Turkish Alphabet unique letters
Yer_update = {"?ORUM": "CORUM", "K?TAHYA": "KUTAHYA", "EGE DENiZi": "EGE DENIZI",
              "DiYARBAKIR": "DIYARBAKIR", "T?RKiYE-iRAN SINIR B?LGESi": "TURKIYE-IRAN SINIR BOLGESI",
              "BALIKESiR ": "BALIKESIR", "SiVAS": "SIVAS", "iZMiR": "IZMIR", "TUNCELi": "TUNCELI",
              "SURiYE": "SURIYE", "ESKiSEHiR": "ESKISEHIR", "DENiZLi": "DENIZLI", "BiTLiS": "BITLIS",
              "KiLiS": "KILIS", "VAN G?L?": "VAN GOLU", "?ANKIRI": "CANKIRI",
              "T?RKIYE-IRAN SINIR B?LGESI": "TURKIYE-IRAN SINIR BOLGESI", "MANiSA": "MANISA",
              "AKDENiZ": "AKDENIZ", "G?RCiSTAN": "GURCISTAN", "BiNGOL": "BINGOL", "OSMANiYE": "OSMANIYE",
              "KIRSEHiR": "KIRSEHIR", "MARMARA DENiZi": "MARMARA DENIZI", "ERZiNCAN": "ERZINCAN",
              "BALIKESiR": "BALIKESIR", "GAZiANTEP": "GAZIANTEP", "G?RCISTAN": "GURCISTAN",
              "?ANAKKALE'": "CANAKKALE", "HAKKARi": "HAKKARI", "AFYONKARAHiSAR": "AFYONKARAHISAR",
              "BiLECiK": "BILECIK", "KAYSERi": "KAYSERI", "T?RKiYE-IRAK SINIR B?LGESi": "TURKIYE-IRAK SINIR BOLGESI",
              "KARADENiZ": "KARADENIZ", "T?RKIYE-IRAK SINIR B?LGESI": "TURKIYE-IRAK SINIR BOLGESI",
              "KARAB?K": "KARABUK", "KIBRIS-SADRAZAMK?Y?K": "KIBRIS-SADRAZAMKOY",
              "T?RKIYE-SURIYE SINIR B?LGESI?K": "TURKIYE-SURIYE SINIR BOLGESI", "?ANAKKALE": "CANAKKALE",
              "KIBRIS-SADRAZAMK?Y": "KIBRIS-SADRAZAMKOY", "ERZURUM ": "ERZURUM",
              "T?RKIYE-SURIYE SINIR B?LGESI": "TURKIYE-SURIYE SINIR BOLGESI", "ADANA ": "ADANA", "KUS G?L?": "KUS GOLU",
              "BURDUR ": "BURDUR", "KIBRIS-G?ZELYURT": "KIBRIS-GUZELYURT", "KONYA ": "KONYA",
              "KOCAELI ": "KOCAELI", "AMASYA ": "AMASYA", "KIRSEHIR ": "KIRSEHIR",
              "KIBRIS-KILI?ASLAN": "KIBRIS-KILICASLAN", "KIBRIS-Z?MR?TK?Y": "KIBRIS-ZUMRUTKOY",
              "DENIZLI ": "DENIZLI", "MANISA ": "MANISA", "ULUBAT G?L?": "ULUBAT GOLU",
              "T?RKIYE-ERMENISTAN SINIR B?LGESI": "TURKIYE-ERMENISTAN SINIR BOLGESI",
              "ERZINCAN ": "ERZINCAN", "TOKAT ": "TOKAT", "ARDAHAN ": "ARDAHAN"}
data['Yer'] = data['Yer'].replace(Yer_update)
data['Yer'].head()
```

![5](https://user-images.githubusercontent.com/77057546/186191942-19db90f6-ad06-4bd0-b358-6d8aff2a0fda.png)

# Data Visualization

## Number of Earthquakes Annually

```Python
aa = data['Olus zamani'].value_counts()
aa = aa.resample('Y').sum().to_frame()
fig = px.line(aa, x=aa.index, y='Olus zamani', text='Olus zamani',
              labels={
                  "index": "Year",
                  "Olus zamani": "Number of Earthquakes"
              }
              )
fig.update_traces(marker=dict(line=dict(color='#000000', width=2)))
fig.update_traces(textposition='top center')
fig.update_layout(title_text='Number of Earthquakes Annually',
                  title_x=0.5, title_font=dict(size=30))
fig.show()
```

![newplot (1)](https://user-images.githubusercontent.com/77057546/186192581-92069866-70a8-4a32-a540-75690ab1d89f.png)


## Distribution of the Earthquakes (Annually)

```Python
fig_hist = px.histogram(data_frame=data, x='Olus zamani')
fig_hist.update_layout(title_text='Distribution of the Earthquakes (Annually)',
                       title_x=0.5, title_font=dict(size=32))
fig_hist.update_traces(marker=dict(line=dict(color='#000000', width=2)))
fig_hist.show()
```
![newplot](https://user-images.githubusercontent.com/77057546/186193247-24761843-7179-4ccf-bf82-b5644c0e8154.png)

## Number of Earthquakes due to Location

```Python
Yer_count = data.groupby(pd.Grouper(key='Yer')).size().reset_index(name='count')
fig = px.treemap(Yer_count, path=['Yer'], values='count')
fig.update_layout(title_text='Number of Earthquakes due to Location',
                  title_x=0.5, title_font=dict(size=30)
                  )
fig.update_traces(textinfo="label+value")
fig.show()
```

![6](https://user-images.githubusercontent.com/77057546/186193682-3226f2e0-e5a2-4a23-8bdf-ecd625b1a4f4.png)

## Distribution of the Earthquakes due to Lat and Long (M>5)

```Python

fig = go.Figure(data=[go.Scatter3d(
    x=data['Enlem'],
    y=data['Boylam'],
    z=data[data['xM'] >= 5]['xM'],
    mode='markers+text',
    hovertext=data['Yer'],
    marker=dict(
        size=5,
        color=data['xM'],
        colorscale='Viridis',
        opacity=0.8
    ),
    text=data[data['xM'] >= 5]['xM'],
)])
fig.update_layout(scene=dict(
    xaxis_title='Latitude',
    yaxis_title='Longitude',
    zaxis_title='xM')
)
fig.update_layout(title_text='Distribution of the Earthquakes due to Lat and Long (M>5)',
                  title_x=0.5, title_font=dict(size=22))
fig.update_layout(margin=dict(l=0, r=0, t=0, b=0))
fig.show()

```

![8](https://user-images.githubusercontent.com/77057546/186195152-65553c6c-780e-43f6-abbc-5d5cf100fea2.png)

## Correlation Graph of the Dataset

```Python

plt.figure(figsize=(15, 8))
correlation = sns.heatmap(data.corr(), vmin=-1, vmax=1, annot=True, linewidths=1, linecolor='black')
correlation.set_title('Correlation Graph of the Dataset', fontdict={'fontsize': 24})

```

![9](https://user-images.githubusercontent.com/77057546/186195575-44ab1621-9cf2-45ff-b2bb-249307a39676.png)

## Heatmap of the Earthquakes

```Python
fig = px.density_mapbox(data, lat=data['Enlem'], lon=data['Boylam'], z=data['xM'],
                        center=dict(lat=39.42, lon=35), zoom=4.5,
                        mapbox_style="stamen-terrain",
                        radius=10,
                        opacity=0.5)
fig.update_layout(title_text='Heatmap of the Earthquakes',
                  title_x=0.5, title_font=dict(size=32))
fig.show()
```

![7](https://user-images.githubusercontent.com/77057546/186194378-521dbe2c-9784-4658-9ba6-053e4dec5de3.png)


## Heatmap of the Earthquakes Animated

```Python

fig = px.density_mapbox(data, lat=data['Enlem'], lon=data['Boylam'], z=data['xM'],
                        center=dict(lat=39.42, lon=35), zoom=4.5,
                        mapbox_style="stamen-terrain",
                        radius=15,
                        opacity=0.5,
                        animation_frame=pd.DatetimeIndex(data['Olus zamani']).year)
fig.update_layout(title_text='Heatmap of the Earthquakes (animated)',
                  title_x=0.5, title_font=dict(size=32))
fig.show()

```
https://user-images.githubusercontent.com/77057546/186194528-88f3362e-93dd-488d-ae3a-eed17530dedf.mp4


See you on another project.

## MAY THE FORCE BE WITH YOU!!!
