# EDA-on-Data-Science-Salary-with-python
You can access the files of this project, which analyzes people working in the field of data science according to countries and working wages.

First, we will download the libraries we will use.

For the dataset link of my work: https://www.kaggle.com/datasets/ruchi798/data-science-job-salaries

`!pip install country_converter`

### import
```Python
import country_converter as coco
import pandas as pd
import numpy as np
import seaborn as sns
import plotly.express as px
import matplotlib.pyplot as plt
import pycountry
from wordcloud import WordCloud
import warnings;
warnings.filterwarnings('ignore')
```

### Reading the DataSet.
```Python
df.head()
```

![image](https://user-images.githubusercontent.com/63750425/184498143-3e76fc69-013b-4e61-818b-bf93c39f8038.png)

`!pip install pycountry`

## Converting country codes to country names for employee_residence and company_location
```Python
resi_country_list = []
comp_country_list = []
for country_code in df.employee_residence:
    resi_country_list.append(pycountry.countries.get(alpha_2=country_code).name)

for country_code in df.company_location:
    comp_country_list.append(pycountry.countries.get(alpha_2=country_code).name)

df['employee_residence'] = resi_country_list
df['company_location'] = comp_country_list
```

### Replacing some of the values to understand the graphs clearly
```Python
df.remote_ratio.replace([100,50,0], ['Remote', 'Hybrid' ,'On-site'],inplace = True)
df.experience_level.replace(['EN','MI','SE', 'EX'], ['Entry', 'Mid', 'Senior', 'Executive'], inplace = True)
```

## ANALYZE OF JOB TITLE
```Python
jobs = df.groupby('job_title').size().reset_index().sort_values(by=0,ascending = False)
jobs.head()
```

![image](https://user-images.githubusercontent.com/63750425/184498227-00c9e862-4128-4fbc-a72d-d118b7299297.png)

```Python
figure_size()
sns.set_style("darkgrid")
sns.barplot(x='job_title',y=0,data = jobs[:10],palette = 'magma')
plt.title('Top Jobs Titles')
plt.xlabel('Job Title')
plt.ylabel('Counts')
plt.xticks(rotation=45)
plt.show()
```

![image](https://user-images.githubusercontent.com/63750425/184498246-7263b0f9-e826-4a15-a70d-c5c92c3baf07.png)

```Python
ax2= px.treemap(df,path=['job_title'],title="Top Job Titles")
ax2.show()
```

![image](https://user-images.githubusercontent.com/63750425/184498265-784ea567-e124-4f63-9434-f5a43cd2879e.png)

```Python
px.histogram(df, x=df.job_title.sort_values(), color = 'experience_level', height = 800, barmode = 'group',
             color_discrete_sequence=px.colors.qualitative.Dark24, template = "plotly_white",
             text_auto  = True, title = 'Count of number of people with all experience levels in each job')
```

![image](https://user-images.githubusercontent.com/63750425/184498295-b80b0aea-2166-4198-937b-0ae2ca192e97.png)

### MULTIVARIENT ANALYSIS
```Python
figure_size()
px.scatter(df, x=df.employee_residence.sort_values(), y = df.company_location.sort_values(), color = 'remote_ratio',
           labels ={"x":'Employee Residence', "y":'Company Location', "remote_ratio":'Work Type'},
           color_discrete_sequence=px.colors.qualitative.Light24, template = 'plotly_white',
           title = 'Company Location VS Employee Residence for type of work(Remote, Hybrid or On-site)')
```


### See you in the next project. I wish you healthy days.
