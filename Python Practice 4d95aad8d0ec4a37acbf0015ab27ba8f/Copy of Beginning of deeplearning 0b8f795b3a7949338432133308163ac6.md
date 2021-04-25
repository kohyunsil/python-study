# Copy of Beginning of deeplearning

deeplearning mnist

```python
import tensorflow as tf

mnist = tf.keras.datasets.mnist

(x_train, y_train), (x_test, y_test) =  mnist.load_data()
x_train, x_test = x_train / 255.0, x_test /255.0
```

```python
model = tf.keras.models.Sequential([
    tf.keras.layers.Flatten(input_shape=(28,28)),
    tf.keras.layers.Dense(1000, activation='relu'),
    tf.keras.layers.Dense(10, activation='softmax'),
])

model.compile(optimizer='adam',
             loss='sparse_categorical_crossentropy',
             metrics=['accuracy'])
```

```python
model.summary()
```

![Copy%20of%20Beginning%20of%20deeplearning%200b8f795b3a7949338432133308163ac6/Untitled.png](Copy%20of%20Beginning%20of%20deeplearning%200b8f795b3a7949338432133308163ac6/Untitled.png)

```python
import time

start_time = time.time()
hist = model.fit(x_train, y_train, validation_data=(x_test, y_test),
                epochs=10, batch_size=100, verbose=1)
print('Fit time :', time.time() - start_time)
```

![Copy%20of%20Beginning%20of%20deeplearning%200b8f795b3a7949338432133308163ac6/Untitled%201.png](Copy%20of%20Beginning%20of%20deeplearning%200b8f795b3a7949338432133308163ac6/Untitled%201.png)

```python
import matplotlib.pyplot as plt
%matplotlib inline
```

```python
plot_target = ['loss', 'val_loss', 'accuracy', 'val_accuracy']
plt.figure(figsize=(12,8))

for each in plot_target:
    plt.plot(hist.history[each], label=each)
    
plt.legend()
plt.grid()
plt.show()
```

![Copy%20of%20Beginning%20of%20deeplearning%200b8f795b3a7949338432133308163ac6/Untitled%202.png](Copy%20of%20Beginning%20of%20deeplearning%200b8f795b3a7949338432133308163ac6/Untitled%202.png)

```python
score = model.evaluate(x_test, y_test)
print('Test loss : ', score[0])
print('Test accuracy : ', score[1])
```

```python
import numpy as np
predicted_result = model.predict(x_test)
predicted_labels = np.argmax(predicted_result, axis=1)
predicted_labels[:10]
```

![Copy%20of%20Beginning%20of%20deeplearning%200b8f795b3a7949338432133308163ac6/Untitled%203.png](Copy%20of%20Beginning%20of%20deeplearning%200b8f795b3a7949338432133308163ac6/Untitled%203.png)

```python
y_test[:10]

wrong_result = []
for n in range(0, len(y_test)):
    if predicted_labels[n] != y_test[n]:
        wrong_result.append(n)
len(wrong_result) 
```

![Copy%20of%20Beginning%20of%20deeplearning%200b8f795b3a7949338432133308163ac6/Untitled%204.png](Copy%20of%20Beginning%20of%20deeplearning%200b8f795b3a7949338432133308163ac6/Untitled%204.png)

```python
import random

samples = random.choices(population=wrong_result, k=16)
samples
```

```python
plt.figure(figsize=(14,12))

for idx, n in enumerate(samples):
    plt.subplot(4, 4, idx+1)
    plt.imshow(x_test[n].reshape(28,28), cmap='Greys', interpolation='nearest')
    plt.title('Label : ' + str(y_test[n]) + 'Predict:' +str(predicted_labels[n]))
    plt.axis('off')
    
plt.show()
```

![Copy%20of%20Beginning%20of%20deeplearning%200b8f795b3a7949338432133308163ac6/Untitled%205.png](Copy%20of%20Beginning%20of%20deeplearning%200b8f795b3a7949338432133308163ac6/Untitled%205.png)