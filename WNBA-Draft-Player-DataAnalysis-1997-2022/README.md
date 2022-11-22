# WNBA Draft Player Data Analysis (1997-2022)

In this project, the statistics of the players in the WNBA drafts from 1997 to 2022 were examined. The data in the dataset, which you can find in the repo, was first organized using data cleaning algorithms. These cleaned data were then graphically extracted using data visualization algorithms.


# Importing Libraries
```Python
import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)
import plotly.express as px #graphing
import matplotlib.pyplot as plt #graphing
import seaborn as sns #graphing
from wordcloud import WordCloud, STOPWORDS # wordcloud
import missingno as msno #describe data
import os
```

# Read Dataset(head)

```Python
data.head()
```

![1](https://user-images.githubusercontent.com/77057546/187431570-fd4f9f70-89dd-4878-846d-205540101cef.png)

```Python
df.describe().style.background_gradient(cmap = "Purples")
```
![2](https://user-images.githubusercontent.com/77057546/187431864-04048ea7-a122-4d62-a455-4b60289fcf86.png)

## Points Per Game by Overall Pick 1-40

```Python
df0 = df[df["overall_pick"] <= 40]

fig = px.scatter_3d(df0, x = "year", y = "overall_pick", z = "points",
                    labels = {"overall_pick": "Pick"}, hover_data = ["player"],
                    color = "overall_pick", color_continuous_scale = "Sunset")

fig.update_traces(marker = dict(size = 3.5, symbol = "circle-open"))
fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 12, color = "#FFB3FF"))
fig.show()
```

![3](https://user-images.githubusercontent.com/77057546/187432302-fc0c8c97-8a5c-4dee-b906-17c76a472a61.png)

## Minutes Played Per Game by Overall Pick 1-40

```Python
df0 = df[df["overall_pick"] <= 40]

fig = px.scatter_3d(df0, x = "year", y = "overall_pick", z = "minutes_played",
                    labels = {"overall_pick": "Pick"}, hover_data = ["player"],
                    color = "overall_pick", color_continuous_scale = "Spectral")

fig.update_traces(marker = dict(size = 3.5, symbol = "circle-open"))
fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 12, color = "#FFB3FF"))
fig.show()
```

![4](https://user-images.githubusercontent.com/77057546/187432926-87c0d9b4-d026-4b63-b30e-af42c497c001.png)

## Wordcloud of Colleges with most 1-10 Overall Picks

```Python
df0 = df[df["overall_pick"] <= 10]
college = df0.college.astype(str)
plt.subplots(figsize = (14, 14))
stop_words = ["University", "State", "nan", "South", "Tech"]

wordcloud = WordCloud(collocations = False, background_color = "#FFB3FF", stopwords = stop_words,
                      width = 512, height = 384).generate(" ".join(college))

plt.imshow(wordcloud)
plt.axis("off")
plt.savefig("graph.png")

plt.show()
```

![graph](https://user-images.githubusercontent.com/77057546/187433735-d6150982-b76e-40cd-a9fa-7da43aa3c702.png)

## Average Minutes Played by Year and Overall Pick

```Python

fig = px.scatter(df, x = "overall_pick", y = "year", hover_data = ["player", "team"],
                 color = "minutes_played", color_continuous_scale = "Jet",
                 title = f"NBA Draft Minutes Played by Year and Overall Pick")

fig.update_traces(marker = dict(size = 8, symbol = "square")) # scaling the markers
fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 20))
fig.show()
```

![6](https://user-images.githubusercontent.com/77057546/187434169-eb20a56e-0701-4830-9544-1842594d1aca.png)

## Win Shares by Overall Pick and Year

```Python
df0 = df[df["games"] > 250]

fig = px.scatter_3d(df0, x = "year", y = "overall_pick", z = "win_shares", 
                    opacity = 0.8, hover_data = ["player"],
                    color = "overall_pick", color_continuous_scale = "Agsunset_r")

fig.update_traces(marker = dict(size = 3.5)) # scaling down the markers
fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 12))
fig.show()
```

![7](https://user-images.githubusercontent.com/77057546/187434494-3f1c9746-c33f-4500-a037-376a3f4a8016.png)


## Overall Picks 1, 2, 3, Average Points

```Python
df0 = df[df["overall_pick"] <= 3]

fig = px.violin(df0, y = "points", points = "all", 
                violinmode = "overlay", hover_data = ["player"],
                title = "Overall Picks 1, 2, 3, Average Points", color = "overall_pick")

fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 20))
fig.show()
```

![8](https://user-images.githubusercontent.com/77057546/187437894-4b3b66cd-238d-4e88-a3b2-8269bd3443b5.png)

## Animation

```Python

df0 = df[df["games"] > 0]

sorted_df = df0.sort_values(by = "overall_pick", ascending = True)

fig = px.scatter_3d(sorted_df, x = "games", y = "year", z = "points",
                    range_x = (0, 1500), range_z = (0, 30),
                    hover_data = ["player"], animation_frame = "overall_pick", 
                    range_color = (0, 25), color = "points", color_continuous_scale = "jet")


fig.update_traces(marker = dict(size = 5)) # scaling down the markers
fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 12))
fig.show()
```

https://user-images.githubusercontent.com/77057546/187438275-18b98c99-c419-4ebc-8783-d303423aa471.mp4

See you on another project.

## MAY THE FORCE BE WITH YOU!!!
