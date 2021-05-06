# Logistic Regression use

data

![Logistic%20Regression%20use%20b6643581aa32416ca7db4a0f0185b5cb/Untitled.png](Logistic%20Regression%20use%20b6643581aa32416ca7db4a0f0185b5cb/Untitled.png)

![Logistic%20Regression%20use%20b6643581aa32416ca7db4a0f0185b5cb/Untitled%201.png](Logistic%20Regression%20use%20b6643581aa32416ca7db4a0f0185b5cb/Untitled%201.png)

```python
sns.distplot(data['Area Income'])
```

![Logistic%20Regression%20use%20b6643581aa32416ca7db4a0f0185b5cb/Untitled%202.png](Logistic%20Regression%20use%20b6643581aa32416ca7db4a0f0185b5cb/Untitled%202.png)

```python
sns.distplot(data['Age'])
```

![Logistic%20Regression%20use%20b6643581aa32416ca7db4a0f0185b5cb/Untitled%203.png](Logistic%20Regression%20use%20b6643581aa32416ca7db4a0f0185b5cb/Untitled%203.png)

```python
data.isna().sum() / len(data)
```

```python
data = data.fillna(round(data['Age'].mean()))
```

![Logistic%20Regression%20use%20b6643581aa32416ca7db4a0f0185b5cb/Untitled%204.png](Logistic%20Regression%20use%20b6643581aa32416ca7db4a0f0185b5cb/Untitled%204.png)

```python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y,
                                                   test_size=0.2, random_state=13)
```

```python
from sklearn.linear_model import LogisticRegression
model = LogisticRegression()
model.fit(X_train, y_train)

model.coef_
```

![Logistic%20Regression%20use%20b6643581aa32416ca7db4a0f0185b5cb/Untitled%205.png](Logistic%20Regression%20use%20b6643581aa32416ca7db4a0f0185b5cb/Untitled%205.png)