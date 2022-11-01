# Finance-Cryptocurrency-Analysis-with-Python
In this project, after extracting the data sets as csv, we tried to reflect them graphically and schematically about cryptocurrencies in finance by using data analysis and data visualization methods. In this project, there are other cryptocurrencies in the data set we provided in this project, but if you want, you can do the same after reading them as csv files. We used 3 cryptocurrencies of our choice. These are Ethereum, bitcoin and dogecoin.
## Libraries and Utilities

```Python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
```
### Let's take a look at our top five data

```Python
df_btc.head()
```

![1](https://user-images.githubusercontent.com/77057546/183245584-4415aad9-90fa-469b-a8c8-21983f3208e2.png)

```Python
df = pd.DataFrame({'BTC': df_btc['Close'],
'ETH': df_eth['Close'],
'DOGE': df_doge['Close']})
df.head()
```
![2](https://user-images.githubusercontent.com/77057546/183245627-d0f3cc95-1e89-494c-a0eb-5d018f1939c2.png)

### Cryptocurrency Graph

```Python
import matplotlib.pyplot as plt
plt.style.use('fivethirtyeight')
my_crypto = df
plt.figure(figsize = (12.2, 4.5))
for c in my_crypto.columns.values:
   plt.plot(my_crypto[c], label = c)
plt.title('Cryptocurrency Graph')
plt.xlabel('Days')
plt.ylabel(' Crypto Price ($)')
plt.legend(my_crypto.columns.values, loc= 'upper left')
plt.show()
```

![3](https://user-images.githubusercontent.com/77057546/183245736-39aae06f-170e-4565-9cf8-61c6bed6bd57.png)

### Cryptocurrency Scaled Graph

```Python
#Visualize the scaled data
my_crypto = df_scale
plt.figure(figsize=(12.4, 4.5))
for c in my_crypto.columns.values:
   plt.plot(my_crypto[c], label=c)
plt.title('Cryptocurrency Scaled Graph')
plt.xlabel('Days')
plt.ylabel('Crypto Scaled Price ($)')
plt.legend(my_crypto.columns.values, loc = 'upper left')
plt.show()
```
![4](https://user-images.githubusercontent.com/77057546/183245806-8630c99a-ccc7-4f7a-b030-bd5549c1e457.png)

### Daily Simple Returns

```Python
plt.figure(figsize=(12, 4.5))
for c in DSR.columns.values:
   plt.plot(DSR.index, DSR[c], label = c, lw = 2, alpha = .7)
plt.title('Daily Simple Returns')
plt.ylabel('Percentage (in decimal form')
plt.xlabel('Days')
plt.legend(DSR.columns.values, loc= 'upper right')
plt.show()
```
![5](https://user-images.githubusercontent.com/77057546/183245879-63dacefe-20cc-4090-b32e-51f8bd854772.png)

### Heat Map

```Python
import seaborn as sns
plt.subplots(figsize= (11,11))
sns.heatmap(DSR.corr(), annot= True, fmt= '.2%')
```

![6](https://user-images.githubusercontent.com/77057546/183245943-3e82bad3-1d8a-4b5f-8b8c-9c9b9b376c54.png)

### Daily Cumulative Simple Return

```Python
plt.figure(figsize=(12.2, 4.5))
for c in DCSR.columns.values:
  plt.plot(DCSR.index, DCSR[c], lw=2, label= c)
plt.title('Daily Cumulative Simple Return')
plt.xlabel('Days')
plt.ylabel('Growth of $1 investment')
plt.legend(DCSR.columns.values, loc = 'upper left', fontsize = 10)
plt.show()
```
![7](https://user-images.githubusercontent.com/77057546/183245991-53b463e7-1509-4524-ab88-32868ec94863.png)
