# Beginning of deeplearning

deeplearning

```python
import numpy as np

raw_data = np.genfromtxt('datas/x09.txt', skip_header=36)
raw_data
```

```python
from mpl_toolkits.mplot3d import Axes3D
import matplotlib.pyplot as plt
%matplotlib inline

xs = np.array(raw_data[:,2], dtype=np.float32)
ys = np.array(raw_data[:,3], dtype=np.float32)
zs = np.array(raw_data[:,4], dtype=np.float32)

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
ax.scatter(xs, ys, zs)
ax.set_xlabel('Weight')
ax.set_ylabel('Age')
ax.set_zlabel('Blood fat')
ax.view_init(15,15)
plt.show()
```

```python
x_data = np.array(raw_data[:, 2:4], dtype=np.float32)
y_data = np.array(raw_data[:,4], dtype=np.float32)

y_data = y_data.reshape((25,1))

model = tf.keras.models.Sequential([
    tf.keras.layers.Dense(1, input_shape=(2, )),
])

model.compile(optimizer='rmsprop', loss='mse')
```

![Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled.png](Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled.png)

```python
model.summary()
```

![Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled%201.png](Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled%201.png)

```python
hist = model.fit(x_data, y_data, epochs=5000)
```

![Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled%202.png](Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled%202.png)

```python
plt.plot(hist.history['loss'])
plt.title('model loss')
plt.ylabel('loss')
plt.xlabel('epochs')
plt.show()

#loss 가 잘 떨어진다.
```

![Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled%203.png](Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled%203.png)

```python
model.predict(np.array([100,44]).reshape(1,2))

model.predict(np.array([60,25]).reshape(1,2))

# 가중치와 bias 확인

W_, b_ = model.get_weights()
print("Weight is : ", W_)
print("bias is : ", b_)
```

```python
x = np.linspace(20, 100, 50).reshape(50,1)
y = np.linspace(10, 70, 50).reshape(50,1)

X = np.concatenate((x, y), axis=1)
Z = np.matmul(X, W_) + b_

fig = plt.figure(figsize=(12,12))
ax = fig.add_subplot(111, projection='3d')
ax.scatter(xs, ys, zs)
ax.scatter(x, y, Z)
ax.set_xlabel('Weight')
ax.set_ylabel('Age')
ax.set_zlabel('Blood fat')
ax.view_init(15,15)
plt.show()
```

![Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled%204.png](Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled%204.png)

XOR

```python
import numpy as np

X = np.array([ [0, 0],
               [1, 0],
               [0, 1],
               [1, 1]])
y = np.array([[0], [1], [1], [0]])
```

```python
model = tf.keras.Sequential([
    tf.keras.layers.Dense(2, activation='sigmoid', input_shape=(2,)),
    tf.keras.layers.Dense(1, activation='sigmoid')
])

##
model.compile(optimizer=tf.keras.optimizers.SGD(lr=0.1), loss='mse')

##
model.summary()

```

![Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled%205.png](Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled%205.png)

```python
model.predict(X)
```

```python
import matplotlib.pyplot as plt
%matplotlib inline

plt.plot(hist.history['loss'])
plt.title('model loss')
plt.ylabel('loss')
plt.xlabel('epochs')
plt.show()
```

![Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled%206.png](Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled%206.png)

```python
for w in model.weights:
    print('---')
    print(w)
```

```python
from sklearn.datasets import load_iris

iris = load_iris()

X = iris.data
y = iris.target
```

OneHotEncoder

```python
from sklearn.preprocessing import OneHotEncoder

enc = OneHotEncoder(sparse=False, handle_unknown='ignore')
enc.fit(y.reshape(len(y), 1))

##
enc.categories_
```

```python
y_onehot = enc.transform(y.reshape(len(y), 1))
y_onehot
```

![Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled%207.png](Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled%207.png)

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y_onehot, test_size=0.2,
                                                   random_state=13)
```

```python
model = tf.keras.models.Sequential([
    tf.keras.layers.Dense(32, input_shape=(4, ), activation='relu'),
    tf.keras.layers.Dense(32, activation='relu'),
    tf.keras.layers.Dense(32, activation='relu'),
    tf.keras.layers.Dense(3, activation='softmax'),
    
])
```

```python
model.compile(optimizer='adam', loss='categorical_crossentropy',
             metrics=['accuracy'])
model.summary()
```

![Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled%208.png](Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled%208.png)

```python
model.evaluate(X_test, y_test, verbose=2)
```

![Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled%209.png](Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled%209.png)

```python
plt.plot(hist.history['loss'])
plt.plot(hist.history['accuracy'])
plt.title('model loss')
plt.ylabel('loss')
plt.xlabel('epochs')
plt.show()
```

![Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled%2010.png](Beginning%20of%20deeplearning%20cb0f539de2e943f08e46a4761f7e1a8b/Untitled%2010.png)