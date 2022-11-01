# Olympics-Data-Analysis-with-Python
I will examine the Data Analysis of the Olympics between 1896-2016, which we have done on Python.

First of all, we define the libraries that we will use in these first four lines of code.
### İmport

```Python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt 
import seaborn as sns 
%matplotlib inline
```
## Load dataset 	

```Python
athletes = pd.read_csv('C:/Users/Yusuf/Desktop/Olympics Data Analysis/athlete_events.csv')
region = pd.read_csv('C:/Users/Yusuf/Desktop/Olympics Data Analysis/noc_regions.csv')
```
We display the csv files of the data we will use by specifying the location on the computer thanks to the code blocks you have seen above.
```Python
athletes.head() 
```

![image](https://user-images.githubusercontent.com/63750425/163809895-31307a55-e3eb-4c50-b6df-e05b891a07eb.png)

```Python
region.head()
```

![image](https://user-images.githubusercontent.com/63750425/163810006-7feb9152-7bbd-472e-a22b-ce3c2034a986.png)

## Join the dataframes
```Python
athletes_df = athletes.merge(region, how = 'left', on = 'NOC')
athletes_df.head()
```

![image](https://user-images.githubusercontent.com/63750425/163810231-e4121545-bda2-449a-b126-7afd973c31c1.png)

In these code blocks, we first displayed the countries where the athletes were located, and finally the region they were in, the names of the athletes, the year they competed, in which branch they competed, and their gender as a graphic.

```Python
athletes_df.shape
```

## Column names consistent 
```Python
athletes_df.rename(columns={'region':'Region','notes':'Notes'},inplace=True );
```
```Python
athletes_df.head()
```
![image](https://user-images.githubusercontent.com/63750425/163810407-237bda5c-b72f-4003-aea9-5775d5065a17.png)

```Python
athletes_df.describe() 
```

![image](https://user-images.githubusercontent.com/63750425/163810482-217f0fdb-63d1-41ec-8876-4e412d452dea.png)
```Python
nan_values = athletes_df.isna()
nan_columns =nan_values.any()
nan_columns
```
```Python
athletes_df.isnull().sum()
```

## India Details
```Python
athletes_df.query('Team == "India"').head(5)
```

![image](https://user-images.githubusercontent.com/63750425/163810667-ef69a9cb-f61d-40f5-a6eb-4ac7fdb82144.png)

## Top Countries Participating
```Python
top_10_countries = athletes_df.Team.value_counts().sort_values(ascending=False).head(10)
top_10_countries
```
```Python
#Plot  forthe top 10 countries 
plt.figure(figsize=(12,6))
#plt.xticks(rotation=20)
plt.title('Overall Participation by Country')
sns.barplot(x=top_10_countries.index, y=top_10_countries, palette = 'Set2' );
```

![image](https://user-images.githubusercontent.com/63750425/163810891-9c6c1e0a-c35f-40ac-9361-be1af891ab61.png)

Here, after determining the 10 countries with the highest number of participants in the code blocks, we showed them graphically.

## Age Distribution of the participants
```Python
plt.figure(figsize=(12,6))
plt.title("Age distribution of the athletes")
plt.xlabel('Age')
plt.ylabel('Number of participants')
plt.hist(athletes_df.Age,bins = np.arange(10,80,2),color='red',edgecolor = 'white');
```

![image](https://user-images.githubusercontent.com/63750425/163811029-f842c779-5773-4153-900c-c5548bb1f2f8.png)

Here, we have shown how many people are in age ranges with a histographic graph.
```Python
winter_sports = athletes_df[athletes_df.Season == 'Winter'].Sport.unique()
winter_sports
```
```Python
#Summer Olympics Sports
summer_sports = athletes_df[athletes_df.Season == 'Summer'].Sport.unique()
summer_sports
```
We used the above codes to view the games played in summer and winter.
```Python
#Male and Female participants
gender_counts = athletes_df.Sex.value_counts()
gender_counts
```
```Python
#Pie plot for male and female athletes
plt.figure(figsize=(12,6))
plt.title('Gender Distribution')
plt.pie(gender_counts,labels=gender_counts.index, autopct='%1.1f%%', startangle=150, shadow=True);
```

![image](https://user-images.githubusercontent.com/63750425/163811309-71173301-dfd9-44e0-8241-fc99cb4a1ea8.png)

Plotly is a technical computing company headquartered in Montreal, Quebec, that develops online data analytics and visualization tools. Plotly provides online graphing, analytics, and statistics tools for individuals and collaboration, as well as scientific graphing libraries for Python, R, MATLAB, Perl, Julia, Arduino, and REST.
Here, we used the plotly library to graphically see the total male-female ratio in the Olympics.
```Python
#Total Medals 
athletes_df.Medal.value_counts()
```
```Python
# Totall number of female athletes in each olympics	
female_participants = athletes_df[(athletes_df.Sex=='F') & (athletes_df.Season=='Summer')][['Sex','Year']]
female_participants = female_participants.groupby('Year').count().reset_index()
female_participants.head()
```
```Python
# Totall number of female athletes in each olympics
female_participants = athletes_df[(athletes_df.Sex=='F') & (athletes_df.Season=='Summer')][['Sex','Year']]
female_participants = female_participants.groupby('Year').count().reset_index()
female_participants.tail()
```

In the code blocks you have seen above, we see the number of women competing by years, first with the head command, and the close years with the tail command.
```Python
womenOlympics = athletes_df[(athletes_df.Sex == 'F') & (athletes_df.Season == 'Summer')]
```
```Python
sns.set(style="darkgrid")
plt.figure(figsize=(20,10))
sns.countplot(x='Year', data=womenOlympics,palette="Spectral")
plt.title('Women Participation') 
```

![image](https://user-images.githubusercontent.com/63750425/163811611-d964650a-74b4-42cb-ac45-c2a8425f7c85.png)

In the graph you have seen above, you can see the increase in female participants over the years. We did this using the plotly library.
```Python
part = womenOlympics.groupby('Year')['Sex'].value_counts()
plt.figure(figsize=(20,10))
part.loc[:,'F'].plot()
plt.title('Plotof Female Athletes over time')
```
![image](https://user-images.githubusercontent.com/63750425/163811709-32e46cf3-16c7-424d-9ac6-d851fbc4187d.png)

In the graph you see above, you can see the increase in female participants over the years. In this graph, we have taken the values linearly. We did this using the plotly library.
```Python
#Gold Medal Athletes
goldMedals = athletes_df[(athletes_df.Medal == 'Gold')]
goldMedals.head()
```
```Python
#take only the alues that are different from nan
goldMedals = goldMedals[np.isfinite(goldMedals['Age'])]
```
```Python
#Gold beyond 60 
goldMedals['ID'][goldMedals['Age'] > 60].count()
```
```Python
sporting_event = goldMedals['Sport'][goldMedals['Age']>60]
sporting_event
```

In the code blocks above, we first listed the gold medal winners, and then got the gold medal winners over the age of 60 and in which branches they did this on the screen.
```Python
# Plot for spoerting_event
plt.figure(figsize=(10,5))
plt.tight_layout()
sns.countplot(sporting_event)
plt.title('Gold Medals for Athletes over 60 years')
```
![image](https://user-images.githubusercontent.com/63750425/163812108-4bbcc305-e93c-4ab0-b763-defcb7c87386.png)

```Python
# Gold medals from each country 
goldMedals.Region.value_counts().reset_index(name='Medal').head(5) 
```

In the code blocks above, we captured the gold medal winners over the age of 60 and in which branches they did this. Afterwards, we used the plotly library to graphically show how many medals they received in which areas.

```Python
totalgoldMedals = goldMedals.Region.value_counts().reset_index(name='Medal').head(6)
2.	g = sns.catplot(x="index", y="Medal", data=totalgoldMedals,height=5, kind="bar",palette="rocket")
3.	g.despine(left=True)
4.	g.set_xlabels("Top 5 Countries")
5.	g.set_ylabels("Number ofMedals")
6.	plt.title('Gold Medals per Country')
```

![image](https://user-images.githubusercontent.com/63750425/163812223-bd0c6e68-c136-4f0d-b1ce-44098e56721a.png)

In the code blocks above, after listing the countries that won the most gold medals, we showed the top 5 countries and the number of medals on the graph.
```Python
#Rio Olympics 
max_year = athletes_df.Year.max()
print(max_year)
team_names = athletes_df[(athletes_df.Year == max_year) & (athletes_df.Medal == 'Gold')].Team	
team_names.value_counts().head(10)
```
```Python
sns.barplot(x=team_names.value_counts().head(20), y=team_names.value_counts().head(20).index)
plt.ylabel(None);
plt.xlabel('Countrywise Medals forthe year 2016')
```

![image](https://user-images.githubusercontent.com/63750425/163812364-f37e2eeb-bcc3-4c92-825b-e8e9994a67de.png)

The above code blocks were first drawn numerically and then graphically, using the plotly library of medal numbers in the 2016-Rio Olympics.

```Python 
not_null_medals = athletes_df[(athletes_df['Height'].notnull()) & (athletes_df['Weight'].notnull())]
```
```Python 
plt.figure(figsize=(12,10))
axis = sns.scatterplot(x="Height", y="Weight", data=not_null_medals, hue= "Sex")
plt.title("Height vs Wight of  Olympic Medalists")
```

![image](https://user-images.githubusercontent.com/63750425/163812486-8a9428af-d79c-4ef7-811c-38a850c06510.png)

In the code blocks above, the weight and height of the competitors are shown on the graph as male and female

