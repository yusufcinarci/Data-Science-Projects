# NBA Draft Player Data Analysis 1989-2021

In this project, the statistics of the players in the NBA drafts from 1989 to 2021 were examined. The data in the dataset, which you can find in the repo, was first organized using data cleaning algorithms. These cleaned data were then graphically extracted using data visualization algorithms.


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
![1](https://user-images.githubusercontent.com/77057546/187243516-e9df2faa-0e0f-4e21-90f0-9c7a2d437a4b.png)

## Total Points Scored by Overall Pick and Year


```Python
df0 = df[df["games"] > 250]

fig = px.scatter_3d(df0, x = "year", y = "overall_pick", z = "points", 
                    opacity = 0.75, hover_data = ["player"],
                    color = "overall_pick", color_continuous_scale = "haline_r")

fig.update_traces(marker = dict(size = 3.5)) # scaling down the markers
fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 12))
fig.show()
```

![2](https://user-images.githubusercontent.com/77057546/187245172-71f25c92-b821-4162-97d1-8be97dc8c7ec.png)

## Breakdown of Pick 1-60

```Python
for y in ["minutes_played", "games", "years_active", "points", "win_shares", "value_over_replacement"]:
    
    fig = px.bar(df, x = "overall_pick", y = y, title = f"NBA Draft Overall Pick 1-60 Total: {y}",
    color = "overall_pick", color_continuous_scale = "Thermal_r",)

    fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 18))
    fig.show()
```

![3](https://user-images.githubusercontent.com/77057546/187245743-549339f5-cdaf-4dc4-bd63-0cd8091a2dea.png)


## Points Per Game by Year and Overall Pick

```Python
fig = px.scatter(df, x = "overall_pick", y = "year", hover_data = ["player", "team"],
                 color = "points_per_game", color_continuous_scale = "Jet",
                 title = f"NBA Draft Points Per Game by Year and Overall Pick")

fig.update_traces(marker = dict(size = 8, symbol = "square")) # scaling the markers
fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 20))
fig.show()
```

![4](https://user-images.githubusercontent.com/77057546/187245914-d3133d5e-5ad2-4977-afde-a0252536a07c.png)

## Where were the best NBA scorers drafted?


```Python
sorted_df = df.sort_values(by = "points", ascending = False)

sorted_df = sorted_df.head(100)

fig = px.bar(sorted_df, x = "player", y = "points", 
             title = "Top 100 NBA Draft Scorers by Overall Pick (Hover for Player Data)",
             color = "overall_pick", color_continuous_scale = "Jet")

#fig.update_traces(texttemplate='%{text:.2s}', textposition='outside')
#fig.update_layout(uniformtext_minsize=8, uniformtext_mode='hide')
fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 20))
fig.update_xaxes(showticklabels = False) # Hide x axis ticks
fig.show()
```

![5](https://user-images.githubusercontent.com/77057546/187246124-962d5396-3d5e-407c-aac9-9b4cccd9f862.png)

## Average Points Per Game for Top 3 Overall Picks

```Python
df0 = df[df["overall_pick"] <= 3]

fig = px.violin(df0, y = "points_per_game", points = "all", 
                violinmode = "overlay", hover_data = ["player"],
                title = "Overall Picks 1, 2, 3, Average Points Per Game", color = "overall_pick")

fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 20))
fig.show()
```

![6](https://user-images.githubusercontent.com/77057546/187246303-4f9b3f28-f37a-4d4e-a984-1c02c00686d8.png)

## Box Plus Minus by Overall Pick

```Python
df0 = df[df["games"] > 0]

sorted_df = df0.sort_values(by = "box_plus_minus", ascending = False)

fig = px.bar(sorted_df, x = "rank", y = "box_plus_minus",
             hover_data = ["player", "team"],
             title = "Box Plus Minus by Overall Pick (Hover for Player Data)",
             color = "box_plus_minus", color_continuous_scale = "Jet")

fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 20))
fig.update_xaxes(showticklabels = False) # Hide x axis ticks
fig.show()
```

![7](https://user-images.githubusercontent.com/77057546/187246541-28f16348-75a2-4bff-a056-7b094ddbad5e.png)


## Value Over Replacement by Overall Pick

```Python
df0 = df[df["games"] > 0]

sorted_df = df0.sort_values(by = "value_over_replacement", ascending = False)

fig = px.bar(sorted_df, x = "rank", y = "value_over_replacement",
             hover_data = ["player", "team"],
             title = "Value Over Replacement by Overall Pick (Hover for Player Data)",
             color = "value_over_replacement", color_continuous_scale = "Jet")

fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 20))
fig.update_xaxes(showticklabels = False) # Hide x axis ticks
fig.show()
```

![8](https://user-images.githubusercontent.com/77057546/187246729-35f21301-471b-4571-86a6-00824d8e25e6.png)

## Points Per Game Overall Pick 1-60 Animation by Year and Games

```Python

df0 = df[df["games"] > 0]

sorted_df = df0.sort_values(by = "overall_pick", ascending = True)

fig = px.scatter_3d(sorted_df, x = "games", y = "year", z = "points_per_game",
                    range_x = (0, 1500), range_z = (0, 30),
                    hover_data = ["player"], animation_frame = "overall_pick", 
                    range_color = (0, 25), color = "points_per_game", color_continuous_scale = "jet")

fig.update_traces(marker = dict(size = 5)) # scaling down the markers
fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 12))
fig.show()
```

https://user-images.githubusercontent.com/77057546/187247740-7a3bdc9d-4233-406c-a385-37bf602073be.mp4
