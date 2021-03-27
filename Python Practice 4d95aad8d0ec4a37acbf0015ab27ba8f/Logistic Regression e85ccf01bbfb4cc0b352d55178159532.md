# Logistic Regression

Logistic Function

```python
import numpy as np

z = np.arange(-10, 10, 0.01)
g = 1 / (1+np.exp(-z))
```

```python
import matplotlib.pyplot as plt
%matplotlib inline

plt.plot(z, g);
```

![Logistic%20Regression%20e85ccf01bbfb4cc0b352d55178159532/Untitled.png](Logistic%20Regression%20e85ccf01bbfb4cc0b352d55178159532/Untitled.png)

```python
plt.figure(figsize=(12,8))
ax = plt.gca()

ax.plot(z,g)
ax.spines['left'].set_position('zero')
ax.spines['right'].set_color('none')
ax.spines['bottom'].set_position('center')
ax.spines['top'].set_color('none')

plt.show()
```

![Logistic%20Regression%20e85ccf01bbfb4cc0b352d55178159532/Untitled%201.png](Logistic%20Regression%20e85ccf01bbfb4cc0b352d55178159532/Untitled%201.png)

hypothesis 함수의 결과에 따른 분류

```python
# Logistic Reg. Cost Fcn의 그래프

h = np.arange(0.01, 1, 0.01)

C0 = -np.log(1-h)
C1 = -np.log(h)

plt.figure(figsize=(12,8))
plt.plot(h, C0, label='y=0')
plt.plot(h, C1, label='y=1')
plt.legend()
plt.show()
```

![Logistic%20Regression%20e85ccf01bbfb4cc0b352d55178159532/Untitled%202.png](Logistic%20Regression%20e85ccf01bbfb4cc0b352d55178159532/Untitled%202.png)

와인데이터 실습

데이터받기

```python
import pandas as pd

wine_url = 'https://raw.githubusercontent.com/PinkWink/ML_tutorial/master/dataset/wine.csv'
wine = pd.read_csv(wine_url, index_col=0)
wine.head()
```

맛 등급 만들어 넣기

```python
wine['taste'] = [1. if grade>5 else 0. for grade in wine['quality']]

X = wine.drop(['taste', 'quality'], axis=1)
y = wine['taste']

# 데이터 분리
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=13)
```

로지스틱 회귀 테스트

```python
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score

lr = LogisticRegression(solver='liblinear', random_state=13)
lr.fit(X_train, y_train)

y_pred_tr = lr.predict(X_train)
y_pred_test = lr.predict(X_test)

print('Train Acc : ', accuracy_score(y_train, y_pred_tr))
print('Test Acc : ', accuracy_score(y_test, y_pred_test))
```

스케일러 적용해서 파이프라인 구축

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler

estimators = [('scaler', StandardScaler()),
             ('clf', LogisticRegression(solver='liblinear', random_state=13))]

pipe_lr = Pipeline(estimators)
```

fit

```python
pipe_lr.fit(X_train, y_train)
```

상승 효과가 있긴하다

```python
y_pred_tr = pipe.predict(X_train)
y_pred_test = pipe.predict(X_test)

print('Train Acc : ', accuracy_score(y_train, y_pred_tr))
print('Test Acc : ', accuracy_score(y_test, y_pred_test))
```

Decision Tree와의 비교를 위한 작업

```python
from sklearn.tree import DecisionTreeClassifier

wine_tree = DecisionTreeClassifier(max_depth=2, random_state=13)
wine_tree.fit(X_train, y_train)

models = {'logistic regression':pipe_lr, 'decision tree':wine_tree}
```

AUC 그래프를 이용한 모델간 비교

```python
from sklearn.metrics import roc_curve

plt.figure(figsize=(10,8))
plt.plot([0,1], [0,1])
for model_name, model in models.items():
    pred = model.predict_proba(X_test)[:, 1]
    fpr, tpr, thresholds = roc_curve(y_test, pred)
    plt.plot(fpr, tpr, label=model_name)

plt.grid()
plt.legend()
plt.show()
```

![Logistic%20Regression%20e85ccf01bbfb4cc0b352d55178159532/Untitled%203.png](Logistic%20Regression%20e85ccf01bbfb4cc0b352d55178159532/Untitled%203.png)

인디언 당뇨병

```python
# 인디언 당뇨병

import pandas as pd

PIMA_url = 'https://raw.githubusercontent.com/PinkWink/ML_tutorial/master/dataset/diabetes.csv'

PIMA = pd.read_csv(PIMA_url)
PIMA.head()
```

```python
PIMA.info()
```

float 으로 데이터 변환

```python
PIMA = PIMA.astype('float')
PIMA.info()
```

일단 상관관계 확인

```python
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline

plt.figure(figsize=(12,10))
sns.heatmap(PIMA.corr(), cmap="YlGnBu")
plt.show()
```

![Logistic%20Regression%20e85ccf01bbfb4cc0b352d55178159532/Untitled%204.png](Logistic%20Regression%20e85ccf01bbfb4cc0b352d55178159532/Untitled%204.png)

그런데 0이 있다, PIMA 인디언에 대한 정보가 없으므로 평균값으로 대체

```python
(PIMA==0).astype(int).sum()

zero_features = ['Glucose', 'BloodPressure', 'SkinThickness', 'BMI']
PIMA[zero_features] = PIMA[zero_features].replace(0, PIMA[zero_features].mean())
(PIMA==0).astype(int).sum()
```

데이터를 나누고

```python
from sklearn.model_selection import train_test_split

X = PIMA.drop(['Outcome'], axis=1)
y = PIMA['Outcome']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2,
                                                   random_state = 13,
                                                   stratify = y)
```

Pipeline을 만들기

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

estimators = [('scaler', StandardScaler()),
             ('clf', LogisticRegression(solver='liblinear', random_state=13))]

pipe_lr = Pipeline(estimators)
pipe_lr.fit(X_train, y_train)
pred = pipe_lr.predict(X_test)
```

```python
from sklearn.metrics import (accuracy_score, recall_score, precision_score,
                            roc_auc_score, f1_score)

print('Accuracy : ', accuracy_score(y_test, pred))
print('Recall : ', recall_score(y_test, pred))
print('Precision : ', precision_score(y_test, pred))
print('AUC score : ', roc_auc_score(y_test, pred))
print('f1 score : ', f1_score(y_test, pred))
```

![Logistic%20Regression%20e85ccf01bbfb4cc0b352d55178159532/Untitled%205.png](Logistic%20Regression%20e85ccf01bbfb4cc0b352d55178159532/Untitled%205.png)

다변수 방정식의 각 계수값을 확인

```python
#계수값
coeff = list(pipe_lr['clf'].coef_[0])
labels = list(X_train.columns)

coeff
```

![Logistic%20Regression%20e85ccf01bbfb4cc0b352d55178159532/Untitled%206.png](Logistic%20Regression%20e85ccf01bbfb4cc0b352d55178159532/Untitled%206.png)

```python
features = pd.DataFrame({'Features':labels, 'importance':coeff})
features.sort_values(by=['importance'], ascending=True, inplace=True)
features['positive']= features['importance'] > 0
features.set_index('Features', inplace=True)
features['importance'].plot(kind='barh',
                            figsize=(11, 6),
                            color=features['positive'].map({True:'blue', False:'red'}))

plt.xlabel('Importance')
plt.show()
```

![Logistic%20Regression%20e85ccf01bbfb4cc0b352d55178159532/Untitled%207.png](Logistic%20Regression%20e85ccf01bbfb4cc0b352d55178159532/Untitled%207.png)