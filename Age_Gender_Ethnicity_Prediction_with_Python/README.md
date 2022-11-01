# Age Gender Ethnicity Prediction with Python

Through this project, I had the chance to use the tensorflow library for the first time in python. 
In the content of the project, I developed a project that predicts the age, gender and ethnicity of the people in the pictures with the data we get from the dataset on kaggle. Apart from these, you can see certain tables using data visualization algorithms related to certain data.


Dataset Link (Kaggle): https://www.kaggle.com/code/shahraizanwar/age-gender-ethnicity-prediction/data

### import
```Python
import numpy as np 
import pandas as pd
import tensorflow as tf
import tensorflow.keras.layers as L
import matplotlib.pyplot as plt
import plotly.graph_objects as go
import plotly.express as px
from sklearn.model_selection import train_test_split

```
# Distributions


```Python
## normalizing pixels data
data['pixels'] = data['pixels'].apply(lambda x: x/255)

## calculating distributions
age_dist = data['age'].value_counts()
ethnicity_dist = data['ethnicity'].value_counts()
gender_dist = data['gender'].value_counts().rename(index={0:'Male',1:'Female'})

def ditribution_plot(x,y,name):
    fig = go.Figure([
        go.Bar(x=x, y=y)
    ])

    fig.update_layout(title_text=name)
    fig.show()
```

# Age Distribtion

```Python
ditribution_plot(x=age_dist.index, y=age_dist.values, name='Age Distribution')
```

![1](https://user-images.githubusercontent.com/77057546/185917615-6b876232-5949-44a6-a392-3864bc7ba24e.png)

# Ethnicity Distribution

```Python
ditribution_plot(x=ethnicity_dist.index, y=ethnicity_dist.values, name='Ethnicity Distribution')
```

![2](https://user-images.githubusercontent.com/77057546/185917904-867ac0ce-4318-4138-8d6b-2e64e716ccef.png)

# Gender Distribution

```Python
ditribution_plot(x=gender_dist.index, y=gender_dist.values, name='Gender Distribution')
```

![3](https://user-images.githubusercontent.com/77057546/185918262-b82e64cd-64d0-4d76-9c1b-c3fca0b602c8.png)

# Sample Images

```Python
plt.figure(figsize=(16,16))
for i in range(1500,1520):
    plt.subplot(5,5,(i%25)+1)
    plt.xticks([])
    plt.yticks([])
    plt.grid(False)
    plt.imshow(data['pixels'].iloc[i].reshape(48,48))
    plt.xlabel(
        "Age:"+str(data['age'].iloc[i])+
        "  Ethnicity:"+str(data['ethnicity'].iloc[i])+
        "  Gender:"+ str(data['gender'].iloc[i])
    )
plt.show()
```

![4](https://user-images.githubusercontent.com/77057546/185918548-85878894-356f-4b32-be00-b362c756d820.png)

# Model for Gender Prediction

```Python
y = data['gender']

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.22, random_state=37
)
```

Build and train model

```Python
model = tf.keras.Sequential([
    L.InputLayer(input_shape=(48,48,1)),
    L.Conv2D(32, (3, 3), activation='relu', input_shape=(32, 32, 3)),
    L.BatchNormalization(),
    L.MaxPooling2D((2, 2)),
    L.Conv2D(64, (3, 3), activation='relu'),
    L.MaxPooling2D((2, 2)),
    L.Flatten(),
    L.Dense(64, activation='relu'),
    L.Dropout(rate=0.5),
    L.Dense(1, activation='sigmoid')
])

model.compile(optimizer='sgd',
              loss=tf.keras.losses.BinaryCrossentropy(),
              metrics=['accuracy'])


## Stop training when validation loss reach 0.2700
class myCallback(tf.keras.callbacks.Callback):
    def on_epoch_end(self, epoch, logs={}):
        if(logs.get('val_loss')<0.2700):
            print("\nReached 0.2700 val_loss so cancelling training!")
            self.model.stop_training = True
        
callback = myCallback()

model.summary()
```

![5](https://user-images.githubusercontent.com/77057546/185919263-50cf3493-cd6d-41e8-822f-d3bd72ba81a1.png)

```Python
history = model.fit(
    X_train, y_train, epochs=20, validation_split=0.1, batch_size=64, callbacks=[callback]
)
```

![6](https://user-images.githubusercontent.com/77057546/185919471-e7f1c1ea-899b-4814-a822-36fe7287ba17.png)

```Python

fig = px.line(
    history.history, y=['loss', 'val_loss'],
    labels={'index': 'epoch', 'value': 'loss'}, 
    title='Training History')
fig.show()

---------------------------------------
loss, acc = model.evaluate(X_test,y_test,verbose=0)
print('Test loss: {}'.format(loss))
print('Test Accuracy: {}'.format(acc))

```
![Ekran görüntüsü 2022-08-22 152126](https://user-images.githubusercontent.com/77057546/185919814-655a3f17-40ab-4d12-bf2c-c6149548119a.png)

We use the same codes that we used in the age part among other options to make predictions, changing their names.


See you on another project.

## MAY THE POWER BE WITH YOU!!!
