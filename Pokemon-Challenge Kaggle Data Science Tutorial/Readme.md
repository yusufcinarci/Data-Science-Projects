 ## Pokemon- Weedle's Cave Data Science Tutorial
 
 ![image](https://user-images.githubusercontent.com/63750425/196903800-5214a673-62ca-4943-b1d2-0e7c1cf3d0ae.png)

 
 In this project, we have reviewed basic commands and modules for data science beginners. 
 We performed the basic functions of some libraries such as matplotlib, pandas and used the Pokemon dataset.
 
### import
```Python 
import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)
import matplotlib.pyplot as plt
import seaborn as sns  # visualization tool
```
```Python 
data = pd.read_csv('..input/pokemon.csv')
data.head()
```
### Dataset Link ------> https://www.kaggle.com/datasets/terminus7/pokemon-challenge

![image](https://user-images.githubusercontent.com/63750425/196902314-479c3b73-2500-419d-ac0b-7640d7c5fa60.png)

![image](https://user-images.githubusercontent.com/63750425/196902365-d6fe70c2-e637-47d7-bb40-6efad69ea852.png)

```Python 
data.info()
```

![image](https://user-images.githubusercontent.com/63750425/196902642-74a1a36c-ec5f-481e-b79a-4c640c692de8.png)

```Python 
# Line Plot
# color = color, label = label, linewidth = width of line, alpha = opacity, grid = grid, linestyle = sytle of line
data.Speed.plot(kind = 'line', color = 'g',label = 'Speed',linewidth=1,alpha = 0.5,grid = True,linestyle = ':')
data.Defense.plot(color = 'r',label = 'Defense',linewidth=1, alpha = 0.5,grid = True,linestyle = '-.')
plt.legend(loc='upper right')     # legend = puts label into plot
plt.xlabel('x axis')              # label = name of label
plt.ylabel('y axis')
plt.title('Line Plot')            # title = title of plot
plt.show()
```

![image](https://user-images.githubusercontent.com/63750425/196902947-2d3ca8c8-67da-4640-b0f1-9a025023794d.png)

```Python 
# 2 - Filtering pandas with logical_and
# There are only 2 pokemons who have higher defence value than 2oo and higher attack value than 100
data[np.logical_and(data['Defense']>200, data['Attack']>100 )]
```
![image](https://user-images.githubusercontent.com/63750425/196903284-116ffa54-9608-408e-89b4-24701c58204c.png)

![image](https://user-images.githubusercontent.com/63750425/196903421-a63e22dc-33be-4dc4-ba09-391dbeb78441.png)

```Python 
# Firstly lets create 2 data frame
data1 = data.head()
data2= data.tail()
conc_data_row = pd.concat([data1,data2],axis =0,ignore_index =True) # axis = 0 : adds dataframes in row
conc_data_row
```

![image](https://user-images.githubusercontent.com/63750425/196903575-f2a2f0f1-c254-4101-a17d-5565c7d272ae.png)

```Python 
# Plotting all data 
data1 = data.loc[:,["Attack","Defense","Speed"]]
data1.plot()
# it is confusing
```

![image](https://user-images.githubusercontent.com/63750425/196903674-39c8fabc-00d5-4d4c-bd17-cc325dd03012.png)


