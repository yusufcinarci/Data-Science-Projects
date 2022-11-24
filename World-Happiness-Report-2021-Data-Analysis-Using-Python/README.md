# World-Happiness-Report-2021-Data-Analysis-Using-Python
In this project, we tried to reflect the happiness rates of countries graphically and schematically by using data sets of 2021 with data analysis data visualization methods.

## Libraries and Utilities

```Python
import numpy as np
import pandas as pd 
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline
```
### Let's take a look at our top five data

```Python
data.head()
```

![10](https://user-images.githubusercontent.com/77057546/182608946-bda080fc-ae7f-4092-827d-0741eedf08a5.png)

### Plot between Ladder score and GDP

```Python
plt.rcParams['figure.figsize'] = (15,7)
plt.title('Plot between Ladder score and GDP')
sns.scatterplot(x = happy_df.ladder_score, y = happy_df.logged_GDP_per_capita, hue = happy_df.regional_indicator, s = 200);

plt.legend(loc = 'upper left',fontsize = '10')
plt.xlabel('Ladder score')
plt.ylabel('GDP per capita')
```
![11](https://user-images.githubusercontent.com/77057546/182609628-57745652-1443-4941-998e-101afcbefbc6.png)

### GDP by Region

```Python
gdp_region.plot.pie(autopct = '%1.1f%%')
plt.title('GDP by Region')
plt.ylabel('')
```

![12](https://user-images.githubusercontent.com/77057546/182610040-44ac82eb-43ec-406b-b15b-fe94f13abbee.png)

### Correlation Map

```Python
cor = happy_df.corr(method = 'pearson')
f, ax = plt.subplots(figsize = (10,5))
sns.heatmap(cor, mask = np.zeros_like(cor, dtype = np.bool), cmap = 'Blues', square = True, ax = ax)
```
![13](https://user-images.githubusercontent.com/77057546/182610369-bbf23b2f-7122-43d1-beda-804912321104.png)

### Perception of Corruption in varius Regions

```Python
dplt.rcParams['figure.figsize'] = (12,8)
plt.title('Perception of Corruption in varius Regions')
plt.xlabel('Regions', fontsize = 15)
plt.ylabel('Corruption Index ', fontsize = 15)
plt.xticks(rotation = 30,ha='right')
plt.bar(corruption.index, corruption.perceptions_of_corruption)
```
![14](https://user-images.githubusercontent.com/77057546/182610726-4082c34d-997e-45ad-a688-362b7f3643a5.png)

### Top 10 happiest countries Life Expectancy&&Bottom 10 least happy countries Life Expectancy

```Python
fig, axes = plt.subplots(1,2, figsize= (16,6))
plt.tight_layout(pad = 2)
xlabels = top_10.country_name
axes[0].set_title('Top 10 happiest countries Life Expectancy')
axes[0].set_xticklabels(xlabels, rotation = 45, ha = 'right')
sns.barplot(x = top_10.country_name, y = top_10.healthy_life_expectancy, ax = axes[0])
axes[0].set_xlabel('Country Name')
axes[0].set_ylabel('Life expectancy')

xlabels=bottom_10.country_name
axes[1].set_title('Bottom 10 least happy countries Life Expectancy')
axes[1].set_xticklabels(xlabels,rotation = 45, ha ='right')
sns.barplot(x = bottom_10.country_name, y= bottom_10.healthy_life_expectancy, ax = axes[1])
axes[1].set_xlabel('Country Name')
axes[1].set_ylabel('Life expectancy')
```

![15](https://user-images.githubusercontent.com/77057546/182611140-e352b5f7-711c-48f3-bf8b-4f6f1d51dc52.png)

### Freedom to make life choices

```Python
plt.rcParams['figure.figsize'] = (15,7)
sns.scatterplot(x = happy_df.freedom_to_make_life_choices, y = happy_df.ladder_score, hue = happy_df.regional_indicator, s=200)
plt.legend(loc = 'upper left', fontsize = '12')
plt.xlabel('Freedom to make life choices')
plt.ylabel('Ladder score')
```
![16](https://user-images.githubusercontent.com/77057546/182611765-50789eb5-dff9-4a23-973c-7d3c53c5fe58.png)

### top five Country of Countries with Most Perception of Corruption

```Python
country = happy_df.sort_values(by='perceptions_of_corruption').head(10)
plt.rcParams['figure.figsize'] = (12,6)
plt.title('Countries with Most Perception of Corruption')
plt.xlabel('Country', fontsize = 13)
plt.ylabel('Corruption Index', fontsize = 13)
plt.xticks(rotation = 30,ha='right')
plt.bar(country.country_name, country.perceptions_of_corruption)
```
![17](https://user-images.githubusercontent.com/77057546/182613456-1594f929-a4c5-41d8-82e4-7723e6256c59.png)

### last five country of Countries with Most Perception of Corruption

```Python
country = happy_df.sort_values(by='perceptions_of_corruption').tail(10)
plt.rcParams['figure.figsize'] = (12,6)
plt.title('Countries with Most Perception of Corruption')
plt.xlabel('Country', fontsize = 13)
plt.ylabel('Corruption Index', fontsize = 13)
plt.xticks(rotation = 30,ha='right')
plt.bar(country.country_name, country.perceptions_of_corruption)
```
![18](https://user-images.githubusercontent.com/77057546/182613693-37051327-4434-45bc-9a28-9098c170923a.png)

### Corruption vs Hapiness
```Python
plt.rcParams['figure.figsize'] = (15,7)
sns.scatterplot(x = happy_df.ladder_score, y = happy_df.perceptions_of_corruption, hue=happy_df.regional_indicator, s=200)
plt.legend(loc='lower left', fontsize = '14')
plt.xlabel('Ladder score')
plt.ylabel('Corruption')
```
![19](https://user-images.githubusercontent.com/77057546/182613872-ed8ff309-b500-4024-ba7c-c97368affed9.png)

### Hope to see you in another project. Take good care of yourself. 
