# Restaurant Recommender System with Python

In this project, first coffee brands in the world and then these brands in Turkey were examined. The data in the dataset, which you can find in the repository, was first organized using data cleaning algorithms. These cleaned data were then extracted graphically using data visualization algorithms. Then, similar restaurants were listed in the prediction part.

# Importing Libraries
```Python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from warnings import filterwarnings
filterwarnings('ignore')

```

# Read Dataset(head)

```Python
data_names.head()
```
![Ekran görüntüsü 2022-08-27 160722](https://user-images.githubusercontent.com/77057546/187031614-dcdad69c-3d13-4c0b-9544-7a5de7f7526f.png)
)

```Python
data_reviews.head()
```
![2](https://user-images.githubusercontent.com/77057546/187031612-b8572c41-fb79-4f47-8049-621c307b4156.png)


# Merging Two Data Sets

I will merge these two data sets.
After the merging I will have a data set with individual customer reviews and ratings for the restaurants.

```Python
# Renaming the restaurant name column with the same value as in the other data set:
data_reviews = data_reviews.rename(columns={'Restaurant': 'Name'})

# Merging the two data sets:
df = pd.merge(data_reviews, data_names, how='left', on='Name')

# Dropping the columns which I am not going to use:
df.drop(['Reviewer', 'Time', 'Pictures', 'Links', 'Collections'], axis=1, inplace=True)
df.head()
```

![3](https://user-images.githubusercontent.com/77057546/187031670-81f3c4f3-320f-4830-ba3a-4a8451c3a500.png)

## Preparing Cost and Rating Columns

```Python
# Changing cost and rating columns data types:
df['Cost'] = df['Cost'].str.replace(',', '').astype(int)
df['Rating'] = df['Rating'].str.replace('Like', '1').astype(float)
df.info()
```

## Handling Missing Values

```Python
print('Nu of data inputs:', len(df))
print('\nNu of NaN values for each column:\n')
print(df.isnull().sum())
-----------------------------------------------------------
# Examine missing Rating values:
df['Name'][df['Rating'].isnull() == True].value_counts()
-----------------------------------------------------------
print('Mean of Rating for American Wild Wings: ', df['Rating'][df['Name'] == 'American Wild Wings'].mean())
print('Mean of Rating for Arena Eleven: ', df['Rating'][df['Name'] == 'Arena Eleven'].mean())
print('Overall Mean of Ratings: ', df['Rating'].mean())
----------------------------------------------------------
df['Rating'].fillna(4, inplace=True)

# Changing NaN reviews by '-'
df['Review'] = df['Review'].fillna('-')
df.isnull().sum()
```

## Separating Metadata (Reviews and Followers)


```Python
# Filling missing values:
df['Metadata'].fillna('0 Review , 0 Follower', inplace=True)

# Standardizing strings
df['Metadata'] = df['Metadata'].str.replace('Reviews', 'Review')
df['Metadata'] = df['Metadata'].str.replace('Followers', 'Follower')

df['Metadata'][df['Metadata'].str.endswith('w')] = df['Metadata'][df['Metadata'].str.endswith('w')] + ' , - Follower'

# Splitting into two columns
df[['Reviews', 'Followers']] = df['Metadata'].str.split(' , ', expand=True)

# Erasing wording from the columns
df['Reviews'] = df['Reviews'].str.replace('Review', '')
df['Reviews'] = df['Reviews'].str.replace('Posts', '')
df['Reviews'] = df['Reviews'].str.replace('Post', '')

df['Followers'] = df['Followers'].str.replace('Follower', '')
df['Followers'] = df['Followers'].str.replace('-', '0')

# Changing str values to integers
df[['Reviews', 'Followers']] = df[['Reviews', 'Followers']].astype(int)

# Dropping the initial column
df.drop(['Metadata'], axis=1, inplace=True)

# Sorting restaurants with their names and costs
df = df.sort_values(['Name', 'Cost'], ascending=False).reset_index()
df.drop('index', axis=1, inplace=True)
df.head()
```

![4](https://user-images.githubusercontent.com/77057546/187031830-8fb61eb0-68d3-47f9-9e3d-4c1d95607c59.png)

## Creating New Features (Mean of Ratings, Reviews, and Followers)

```Python
restaurants = list(df['Name'].unique())
df['Mean Rating'] = 0
df['Mean Reviews'] = 0
df['Mean Followers'] = 0

for i in range(len(restaurants)):
    df['Mean Rating'][df['Name'] == restaurants[i]] = df['Rating'][df['Name'] == restaurants[i]].mean()
    df['Mean Reviews'][df['Name'] == restaurants[i]] = df['Reviews'][df['Name'] == restaurants[i]].mean()
    df['Mean Followers'][df['Name'] == restaurants[i]] = df['Followers'][df['Name'] == restaurants[i]].mean()

df.sample(3)
```
![5](https://user-images.githubusercontent.com/77057546/187031887-9f36e852-1fb7-4a56-9abc-d6e215e3d62e.png)

## Feature Scaling

```Python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler(feature_range = (1,5))

df[['Mean Rating', 'Mean Reviews', 'Mean Followers']] = scaler.fit_transform(df[['Mean Rating', 'Mean Reviews', 'Mean Followers']]).round(2)

df.sample(3)
```

![6](https://user-images.githubusercontent.com/77057546/187031935-d15fa19f-8995-4d58-96a5-92a4d0e08f7b.png)

# Text Preprocessig and Cleaning

```Python
import re
from nltk.corpus import stopwords
from sklearn.metrics.pairwise import linear_kernel
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.feature_extraction.text import TfidfVectorizer
```

```Python
# Define symbols to be replaced by space
replace_space = re.compile('[/(){}\[\]\|@,;]')
# Define symbols to be removed
remove_symbols = re.compile('[^0-9a-z #+_]')
# Define stopwords
stopwords = set(stopwords.words('english'))

def text_preprocessing(text):
    # Lowercase all the letters
    text = text.lower()
    
    # Replace these symbols with space
    text = replace_space.sub(' ', text)
    
    # Remove these symbols
    text = remove_symbols.sub('', text)
    
    # Remove stopwords
    text = ' '.join(word for word in text.split() if word not in stopwords)
    
    return text
df['Review'] = df['Review'].apply(text_preprocessing)
df['Cuisines'] = df['Cuisines'].apply(text_preprocessing)
# Columns after processed:
df[['Review','Cuisines']].sample(5)
```

![7](https://user-images.githubusercontent.com/77057546/187032003-8d8087ea-7c7f-47e9-a37e-bc32db358a32.png)

# EDA - Analysing Restaurants and Popularities

## Top Rated 10 Restaurants

```Python
df_rating = df.drop_duplicates(subset='Name')
df_rating = df_rating.sort_values(by='Mean Rating', ascending=False).head(10)

plt.figure(figsize=(7,5))
sns.barplot(data=df_rating, x='Mean Rating', y='Name', palette='RdBu')
plt.title('Top Rated 10 Restaurants');
```

![8](https://user-images.githubusercontent.com/77057546/187032076-ef91e1ed-7e0e-43a0-b7b9-f8c2e6adf1ab.png)

## Top Reviewed 10 Restaurants

```Python
df_reviews = df.drop_duplicates(subset='Name')
df_reviews = df_reviews.sort_values(by='Mean Reviews', ascending=False).head(10)

plt.figure(figsize=(7,5))
sns.barplot(data=df_reviews, x='Mean Reviews', y='Name', palette='RdBu')
plt.title('Top Reviewed 10 Restaurants');
```

![9](https://user-images.githubusercontent.com/77057546/187032101-ba109279-3370-405b-a688-475ae6d9190e.png)

## Most Followed Top 10 Restaurants

```Python
df_followers = df.drop_duplicates(subset='Name')
df_followers = df_followers.sort_values(by='Mean Followers', ascending=False).head(10)

plt.figure(figsize=(7,5))
sns.barplot(data=df_followers, x='Mean Followers', y='Name', palette='RdBu')
plt.title('Most Followed Top 10 Restaurants');
```

![10](https://user-images.githubusercontent.com/77057546/187032135-30fe76ae-98aa-4b92-ade3-1ae4f77a4500.png)

# EDA - Word Frequency Distribution:

```Python
def get_top_words(column, top_nu_of_words, nu_of_word):
    
    vec = CountVectorizer(ngram_range= nu_of_word, stop_words='english')
    
    bag_of_words = vec.fit_transform(column)
    
    sum_words = bag_of_words.sum(axis=0)
    
    words_freq = [(word, sum_words[0, idx]) for word, idx in vec.vocabulary_.items()]
    
    words_freq =sorted(words_freq, key = lambda x: x[1], reverse=True)
    
    return words_freq[:top_nu_of_words]
----------------------------------------------------------------------------------
# Top 20 two word frequencies for Cuisines
list1 = get_top_words(df['Cuisines'], 20, (2,2))

df_words1 = pd.DataFrame(list1, columns=['Word', 'Count'])

plt.figure(figsize=(7,6))
sns.barplot(data=df_words1, x='Count', y='Word')
plt.title('Word Couple Frequency for Cuisines');
```

![11](https://user-images.githubusercontent.com/77057546/187032190-57d267e0-e7c3-46b2-aec1-5b429b81488a.png)

# CONTENT BASE RECOMMENDER SYSTEM

```Python
# Changing data set index by restaurant name
df.set_index('Name', inplace=True)

# Saving indexes in a series
indices = pd.Series(df.index)

# Creating tf-idf matrix
tfidf = TfidfVectorizer(analyzer='word', ngram_range=(1, 2), min_df=0, stop_words='english')
tfidf_matrix = tfidf.fit_transform(df['Review'])

# Calculating cosine similarities
cosine_similarities = linear_kernel(tfidf_matrix, tfidf_matrix)
```

# Creating the Recommender System:

```Python
 def recommend(name, cosine_similarities = cosine_similarities):
    
    # Create a list to put top 10 restaurants
    recommend_restaurant = []
    
    # Find the index of the hotel entered
    idx = indices[indices == name].index[0]
    
    # Find the restaurants with a similar cosine-sim value and order them from bigges number
    score_series = pd.Series(cosine_similarities[idx]).sort_values(ascending=False)
    
    # Extract top 30 restaurant indexes with a similar cosine-sim value
    top30_indexes = list(score_series.iloc[0:31].index)
    
    # Names of the top 30 restaurants
    for each in top30_indexes:
        recommend_restaurant.append(list(df.index)[each])
    
    # Creating the new data set to show similar restaurants
    df_new = pd.DataFrame(columns=['Cuisines', 'Mean Rating', 'Cost', 'Timings'])
    
    # Create the top 30 similar restaurants with some of their columns
    for each in recommend_restaurant:
        df_new = df_new.append(pd.DataFrame(df[['Cuisines','Mean Rating', 'Cost', 'Timings']][df.index == each].sample()))
    
    # Drop the same named restaurants and sort only the top 10 by the highest rating
    df_new = df_new.drop_duplicates(subset=['Cuisines','Mean Rating', 'Cost'], keep=False)
    df_new = df_new.sort_values(by='Mean Rating', ascending=False).head(10)
    
    print('TOP %s RESTAURANTS LIKE %s WITH SIMILAR REVIEWS: ' % (str(len(df_new)), name))
    
    return df_new
# HERE IS A RANDOM RESTAURANT. LET'S SEE THE DETAILS ABOUT THIS RESTAURANT:
df[df.index == 'Hyderabadi Daawat'].head(1)    
```

![12](https://user-images.githubusercontent.com/77057546/187032262-cd5c58de-612b-4662-bb6b-8f3c935800e6.png)


```Python
# LET'S SEE WHAT ARE WE GOING TO BE RECOMMENDED:
recommend('Hyderabadi Daawat')
```

![13](https://user-images.githubusercontent.com/77057546/187032311-e54ee648-0a98-438f-9baa-ec4754051ba0.png)
 
```Python
# HERE IS A MEDITERRANEAN / NORT INDIAN / KEBAB / BBQ RESTAURANT. LET'S SEE THE DETAILS ABOUT THIS RESTAURANT:
df[df.index == 'Barbeque Nation'].sample(1)

# LET'S SEE WHAT ARE WE GOING TO BE RECOMMENDED:
recommend('Barbeque Nation')
```
![14](https://user-images.githubusercontent.com/77057546/187032387-a01d1e71-9a65-4bb5-83e9-f11baeb30213.png)
































































































































See you on another project.

## MAY THE FORCE BE WITH YOU!!!
