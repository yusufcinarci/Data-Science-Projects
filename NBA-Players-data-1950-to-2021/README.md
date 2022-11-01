# NBA-Players-data-1950-to-2021

In this project, the data of the NBA players between the years 1950-2021 were examined. After the NBA players' season, height, performance, averages of points, teams and positions they played were obtained through csv files, important tables and graphs were created using data cleaning and data visualization algorithms.

First, we will download the libraries we will use.

### import
```Python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.graph_objs as go
from plotly.offline import init_notebook_mode, iplot
import plotly.figure_factory as ff
import plotly
from plotly import tools
import plotly.express as px
import missingno as mno
sns.set_style("whitegrid", {"grid.color": ".2", "grid.linestyle": ":"})
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score
from scipy.cluster.hierarchy import linkage
from scipy.cluster.hierarchy import dendrogram
from scipy.cluster.hierarchy import cut_tree
```

### After importing our libraries, we defined our dataset.


(https://www.kaggle.com/datasets/amanuelwk/nba-regular-season-data)

### We show the first five data.

```Python 
seasons_stats.head()
```
![1](https://user-images.githubusercontent.com/77057546/181226958-3bea2cc0-900e-4166-9b9c-1403db466507.png)

### Data Cleaning and Initial Preperation
data of all the players from all the season after 3-pointer was introduced
```Python 
df = seasons_stats.loc[seasons_stats['Year'] > float(1979)]
df.head()
```
![2](https://user-images.githubusercontent.com/77057546/181227822-1ce89d10-7e4d-4d75-b61e-1b8a0318bc9d.png)

checking missing values

```Python 
total = df.isnull().sum().sort_values(ascending = False)
percent = (df.isnull().sum()/df.isnull().count()*100).round(2).sort_values(ascending = False)
pd.concat([total, percent], axis=1, keys=['Total Missing', 'Percent Missing'])
```
![3](https://user-images.githubusercontent.com/77057546/181228379-137498c5-2b67-4829-903d-9e59a1a7c332.png)

filtering out cumulative records for each player for every year

```Python 
df = df[~df.duplicated(subset = ['Year', 'Player'], keep = 'first')]
df.head()
```
![4](https://user-images.githubusercontent.com/77057546/181229134-18b253a5-daed-41ee-92d9-88641712caa4.png)


### Basketball Player Positions
```Python
df_2021 = df[df['Year'] == float(2021)]
df_2021.head()
```
![5](https://user-images.githubusercontent.com/77057546/181230081-6eb73c78-d641-4e23-a707-63842c13a559.png)

### Player Performance Metrics (season 2020-2021)
```Python
df_2021.head()
```

![6](https://user-images.githubusercontent.com/77057546/181230543-73cc0cfa-afc8-4f1a-81c4-78235c327930.png)


### Deriving per-match stats 
```Python
df_pmatch = df_2021[['PTS', 'FG', 'FGA', '3P', '3PA', '2P', '2PA', 'FT', 'FTA', 'ORB', 'DRB', 'AST', 'STL', 'BLK']].apply(per_game)
df_pmatch[['Pos','G','FG%', '3P%', '2P%', 'FT%']] = df_2021[['Pos', 'G','FG%', '3P%', '2P%', 'FT%']]
df_pmatch.head()
```

![7](https://user-images.githubusercontent.com/77057546/181230902-238e9e3e-dda6-42e9-9e2b-8e501344d776.png)

### Dimensionality Reduction Techniques (PCA and t-SNE)
Principal component analysis (PCA)

```Python
from sklearn.decomposition import PCA
X = df_m[perf_features]
pca = PCA(n_components=2)
pca.fit(X)
components = pca.transform(X)
pca_df = pd.DataFrame({'component_1': components[:,0], 'component_2': components[:,1]})

# adding Pos column
posi = df_m['Pos'].reset_index()
posi = posi.drop('index', axis = 1)
c12 = pd.concat([pca_df, posi], axis = 1)
c12.head()

plt.figure(figsize = [12, 8])
ax = sns.scatterplot(c12['component_1'], c12['component_2'], hue = c12['Pos'], s = 50, alpha = 0.8)
ax.patch.set_edgecolor('black')
ax.patch.set_linewidth(1.5) 
```
![8](https://user-images.githubusercontent.com/77057546/181231481-257cc7d1-d9ce-49f5-9793-66c500649b30.png)

t - Distributed Stochastic Neighbor Embedding (t-SNE)

```Python
plt.figure(figsize = [12, 8])
ax = sns.scatterplot(c12['component_1'], c12['component_2'], hue = c12['Pos'], s = 50, alpha = 0.8)
ax.patch.set_edgecolor('black')
ax.patch.set_linewidth(1.5) 
```

![9](https://user-images.githubusercontent.com/77057546/181231763-71da8819-08c8-4692-b6c0-e26e17cd99a8.png)

Finding Optimal number of Clusters (k)

```Python
# elbow-curve
ssd = []
range_n_clusters = [2, 3, 4, 5, 6, 7, 8, 9, 10]
for num_clusters in range_n_clusters:
    kmeans = KMeans(n_clusters=num_clusters, max_iter = 500)
    kmeans.fit(pca_df)
    
    ssd.append(kmeans.inertia_)
    
# plot the SSDs for each n_clusters
# ssd

plt.figure(figsize = [12, 12])

plt.subplot(2,1,1)
with plt.style.context('seaborn-whitegrid'):
    sns.lineplot(np.arange(2,11,1), ssd, marker='o', markersize = 10)
    plt.xlabel('number of clusters', fontsize = 14)
    plt.axvline(4, ls="--", c="red")
    plt.ylabel('Total within-sum-of-squares', fontsize = 14)
    plt.title('Cluster Validation Measures for PCA Scatterplot', fontsize = 16, fontweight = 'bold')


# silhouette analysis
range_n_clusters = [2, 3, 4, 5, 6, 7, 8, 9, 10]
sil = []
for num_clusters in range_n_clusters:
    
    # intialise kmeans
    kmeans = KMeans(n_clusters=num_clusters, max_iter = 500)
    kmeans.fit(pca_df)
    
    cluster_labels = kmeans.labels_
    
    # silhouette score
    silhouette_avg = silhouette_score(pca_df, cluster_labels)
    sil.append(silhouette_avg)

plt.subplot(2,1,2)
with plt.style.context('seaborn-whitegrid'):
    sns.lineplot(np.arange(2,11,1), sil, marker='o', markersize = 10)
    plt.yticks(np.arange(0,0.6,0.1))
    plt.xlabel('number of clusters', fontsize = 14)
    plt.axvline(4, ls="--", c="red")
    plt.ylabel('Silhouette score', fontsize = 14)
```

![10](https://user-images.githubusercontent.com/77057546/181232057-c8e701e8-29bb-49f8-8b75-5e4adfa29b2c.png)

In our project above, we tried to show some important and striking graphics. You can find all the files of our project on our data science journey in the github repository.
