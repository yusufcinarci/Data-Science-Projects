# EDA-SpaceX-Launches-Falcon9-and-Falcon-Heavy
In this project, we analyze the space flight data of Spacex space research company Falcon 9 rocket.


![image](https://user-images.githubusercontent.com/63750425/185083005-a6a30ef4-2cc6-4d1e-8be0-0feacb633995.png)

Dataset1 Link (Kaggle): https://www.kaggle.com/datasets/sagarvarandekar/spacex-falcon9-launch-data
Dataset2 Link (Kaggle): https://www.kaggle.com/datasets/heyrobin/space-x-launches-falcon9-and-falcon-heavy

### About the Dataset
Space Exploration Technologies Corp. (SpaceX) is an American aerospace manufacturer, space transportation services.

Falcon 9 is a rocket that can carry cargo and humans into Earth orbit, even reaching the International Space Station. It is produced by American aerospace company SpaceX. Technically, it is a partially reuseable, medium lift launch vehicle.

Falcon Heavy is a partially reusable heavy-lift launch vehicle that is produced by SpaceX, an American aerospace manufacturer.

### import
```Python
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px

#Custom Colors
class clr:
    S = '\033[1m' + '\033[96m'
    E = '\033[0m'
    
my_colors = ["#094074","#3C6997", "#9900F0", "#FFD124", "#FE9000"]

print(clr.S + "Notebook Color Scheme: " + clr.E)
sns.palplot(sns.color_palette(my_colors))
```

```Python
df.describe()
```

![image](https://user-images.githubusercontent.com/63750425/185083629-0fb48238-b48e-476c-b7e2-90a8ef7190bf.png)

```Python
df.head()
```

![image](https://user-images.githubusercontent.com/63750425/185083786-69004259-5af5-46ac-bcb3-392d748f24cd.png)

```Python
launch_site = df['LaunchSite'].value_counts()
launch_site
```


![image](https://user-images.githubusercontent.com/63750425/185083880-17f679cf-0e69-4779-8373-cdd2655af336.png)

```Python
most_orbit = df['Orbit'].value_counts()
most_orbit
```
```Python
plt.figure(figsize = (15,8))
most_orbit.plot(kind = "barh",color = my_colors[1] )
plt.show()
```

![image](https://user-images.githubusercontent.com/63750425/185083994-80b390f4-7b3b-4c88-be12-d2d9d41fd5fd.png)

```Python
plt.figure(figsize = (15,8))
Most_customers.plot(kind = 'bar', color = my_colors[2])
plt.show()
```

![image](https://user-images.githubusercontent.com/63750425/185084091-7c13dc51-767a-429f-aa90-77ba2649164b.png)


```Python
outcomes_of_landing = df1['Booster_landing'].value_counts()
plt.figure(figsize = (15,8))
outcomes_of_landing.plot(kind = 'pie')
plt.show()
```
```Python
plt.figure(figsize=(40,20))
year_club = df1.groupby(['Booster_landing', 'Version, Booster']).Booster_landing.size().head(20).plot(kind = 'pie')
plt.title('Booster_landing VS Version, Booster')
plt.legend()
plt.show()
```

![image](https://user-images.githubusercontent.com/63750425/185084232-28a972c1-810b-4366-9747-8b2bad4f4cf5.png)


See you on another project.

## MAY THE POWER BE WITH YOU!!!

"If humanity doesn't land on Mars in my lifetime, I would be very disappointed."
Elon Musk
