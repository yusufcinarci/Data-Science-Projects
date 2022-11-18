# The-Office-EDA
Data analysis study of my favorite sitcom, The Office (US).

![image](https://user-images.githubusercontent.com/63750425/184617310-53e52864-4370-4981-a9ee-93a0b3954a8b.png)

First, we will download the libraries we will use.
`!pip install pytrends`



### import
```Python
import holoviews as hv
from wordcloud import WordCloud
from pytrends.request import TrendReq
import plotly.express as px
import pandasql as ps
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
import pandas as pd
import os
```

```Python
table = hv.Table(dataset)
table.opts(height=250,width=1200)
```

![image](https://user-images.githubusercontent.com/63750425/184617570-b73a4258-fa1c-45e1-a915-2f45db7bc2c9.png)

```Python
table = hv.Table(dataset.describe().T.reset_index())
table.opts(height=150,width=700)
```

![image](https://user-images.githubusercontent.com/63750425/184617647-64ecf718-4f0d-40f5-961b-d0c80f4a95ba.png)

```Python
fig = px.choropleth(pytrends.interest_by_region(resolution='COUNTRY',inc_geo_code=True).reset_index(),
                                color="The Office", 
                                color_continuous_scale='Blues',
                                locations = "geoName",
                                locationmode="country names",
                                projection="natural earth")
fig.update_layout(margin={"r":0,"t":0,"l":0,"b":0},dragmode=False, coloraxis_showscale=False)
fig.update_geos(fitbounds="locations", visible=False)
fig.show()
print("Figure: Google search trend for The Office")
```

![image](https://user-images.githubusercontent.com/63750425/184617741-2ec86dc6-f791-4880-9656-de22d54766ad.png)

```Python
fig = px.line(pytrends.interest_over_time().iloc[:,:1].reset_index(), 
              x='date', 
              y='The Office')
fig.update_layout(legend_title_text='',paper_bgcolor="white",plot_bgcolor='rgba(0,0,0,0)')
fig.update_yaxes(showgrid=True, gridwidth=1, gridcolor='silver')
fig.update_xaxes(showgrid=True, gridwidth=1, gridcolor='silver')
fig.show()
print("Figure: Google search trend for The Office")
```

![image](https://user-images.githubusercontent.com/63750425/184617793-d32eaa69-a263-448f-a9e0-2cd26d311925.png)


```Python
fig = px.pie(pytrends.related_queries()["The Office"]['top'], values='value', names='query',color_discrete_sequence=px.colors.qualitative.G10)
fig.update_traces(textposition='inside', textinfo='percent+label')
fig.update_layout(legend_title_text='Related Queries')
fig.show()
```

![image](https://user-images.githubusercontent.com/63750425/184617893-41c42e97-3064-4063-9a25-61bf929dcfb0.png)

```Python
text = ""
for words in dataset["About"].str.split(" "):
    for word in words:
        text = text + ' ' + word
wordcloud = WordCloud(width=900, height=400, background_color="#0f4c5c").generate(text)
plt.figure(figsize=(20,10))
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis("off")
plt.margins(x=0, y=0)
plt.show()
```

![image](https://user-images.githubusercontent.com/63750425/184617947-55f3008e-23ee-4872-992d-427a1415399f.png)

```Python
df = dataset[["Season","EpisodeTitle","Ratings"]].sort_values("Ratings",ascending=False).head(20).reset_index(drop=True)
table = hv.Table(df)
table.opts(height=530,width=400)
```

![image](https://user-images.githubusercontent.com/63750425/184617999-9a55c291-2b69-4692-81ff-13576cecdd8b.png)


#### See you in the next project. I wish you healthy days. 

## And finally THAT'S WHAT SHE SAID...
