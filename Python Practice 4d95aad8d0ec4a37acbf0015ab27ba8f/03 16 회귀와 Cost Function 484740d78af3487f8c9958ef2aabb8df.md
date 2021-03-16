# 03/16 회귀와 Cost Function

예제

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

data = pd.read_csv('ecommerce.csv')
```

일단 필요없다고 판단된 컬럼 제거

```python
data.drop(['Email', 'Address', 'Avatar'], axis = 1, inplace = True)
```

데이터나누기

```python
from sklearn.model_selection import train_test_split

X = data.drop('Yearly Amount Spent', axis=1)
y = data['Yearly Amount Spent']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.3, random_state=13)
```

선형회귀 모델 만들기

```python
import statsmodels.api as sm

lm = sm.OLS(y_train, X_train).fit()
lm.summary()
```

![03%2016%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%E1%84%8B%E1%85%AA%20Cost%20Function%20484740d78af3487f8c9958ef2aabb8df/_2021-03-16__12.56.46.png](03%2016%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%E1%84%8B%E1%85%AA%20Cost%20Function%20484740d78af3487f8c9958ef2aabb8df/_2021-03-16__12.56.46.png)

예측

```python
predictions = lm.predict(X_test)
predictions

### 실제 테스트 값
y_test
```

참값과 예측값의 직관적 비교

```python
sns.scatterplot(y_test,predictions);
plt.plot([300,800], [300,800], 'r', ls='dashed', lw=3);
```

연산

```python
!pip install sympy
```

```python
# Symbolic 연산

import sympy as sym

th = sym.Symbol('th')
diff_th = sym.diff(38*th**2 - 94*th + 62, th)
diff_th
```

보스톤 집값 데이터 읽기

```python
from sklearn.datasets import load_boston

boston = load_boston()
print(boston.DESCR)
```

각 특성의 의미

```python
[each for each in boston.feature_names]
```

데잍 파악을 위해 pandas로 정리

```python
import pandas as pd

boston_pd = pd.DataFrame(boston.data, columns=boston.feature_names)
boston_pd['PRICE'] = boston.target

boston_pd.head()
```

집값에 대한 히스토그램

```python
import plotly.express as px

fig = px.histogram(boston_pd, x='PRICE')
fig.show()
```

각 특성별 상관계수 확인

```python
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline

corr_mat = boston_pd.corr().round(1)
sns.set(rc={'figure.figsize':(10,8)})
sns.heatmap(data=corr_mat, annot=True, cmap='bwr');
```

RM과 LSTAT와 PRICE의 관계에 대해 좀 더 관찰

- 저소득층 인구가 낮을 수록, 방의 갯수가 많을 수록 집 값이 높아지는가?

```python
sns.set_style('darkgrid')
sns.set(rc={'figure.figsize':(12,6)})
fig, ax = plt.subplots(ncols=2)
sns.regplot(x='RM', y='PRICE', data=boston_pd, ax=ax[0])
sns.regplot(x='LSTAT', y='PRICE', data=boston_pd, ax=ax[1]);
```

데이터 나누고, LinearResgression 사용

```python
### 데이터 나누기
from sklearn.model_selection import train_test_split

X = boston_pd.drop('PRICE', axis=1)
y = boston_pd['PRICE']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=13)

### LinearResgression
from sklearn.linear_model import LinearRegression

reg = LinearRegression()
reg.fit(X_train, y_train)
```

모델 평가

```python
import numpy as np
from sklearn.metrics import mean_squared_error

pred_tr = reg.predict(X_train)
pred_test = reg.predict(X_test)
rmse_tr = (np.sqrt(mean_squared_error(y_train, pred_tr)))
rmse_test = (np.sqrt(mean_squared_error(y_test, pred_test)))

print('RMSE of Train Data : ', rmse_tr)
print('RMSE of Test Data : ', rmse_test)
```

결과

```python
plt.scatter(y_test, pred_test)
plt.xlabel("Actual House Prices ($1000)")
plt.ylabel("Predicted Prices")
plt.title("Real vs Predicted")
plt.plot([0,48], [0,48], 'r')
plt.show()
```

![03%2016%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%E1%84%8B%E1%85%AA%20Cost%20Function%20484740d78af3487f8c9958ef2aabb8df/Untitled.png](03%2016%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%E1%84%8B%E1%85%AA%20Cost%20Function%20484740d78af3487f8c9958ef2aabb8df/Untitled.png)

LSTAT를 사용하는 것이 맞는가?

```python
X = boston_pd.drop(['PRICE', 'LSTAT'], axis=1)
y = boston_pd['PRICE']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=13)

reg = LinearRegression()
reg.fit(X_train, y_train)

###### 당연히 성능은 나빠짐

pred_tr = reg.predict(X_train)
pred_test = reg.predict(X_test)
rmse_tr = (np.sqrt(mean_squared_error(y_train, pred_tr)))
rmse_test = (np.sqrt(mean_squared_error(y_test, pred_test)))

print('RMSE of Train Data : ', rmse_tr)
print('RMSE of Test Data : ', rmse_test)
```

처음값

![03%2016%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%E1%84%8B%E1%85%AA%20Cost%20Function%20484740d78af3487f8c9958ef2aabb8df/Untitled%201.png](03%2016%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%E1%84%8B%E1%85%AA%20Cost%20Function%20484740d78af3487f8c9958ef2aabb8df/Untitled%201.png)

나중값

![03%2016%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%E1%84%8B%E1%85%AA%20Cost%20Function%20484740d78af3487f8c9958ef2aabb8df/Untitled%202.png](03%2016%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%E1%84%8B%E1%85%AA%20Cost%20Function%20484740d78af3487f8c9958ef2aabb8df/Untitled%202.png)

- RMSE 값은 커질수록 성능이 나빠지는 것을 의미

```python

# 이런 류의 고민은 계속됨..

plt.scatter(y_test, pred_test)
plt.xlabel("Actual House Prices ($1000)")
plt.ylabel("Predicted Prices")
plt.title("Real vs Predicted")
plt.plot([0,48],[0,48], 'r')
plt.show()
```

- 이전 결과 vs 이후 결과

![03%2016%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%E1%84%8B%E1%85%AA%20Cost%20Function%20484740d78af3487f8c9958ef2aabb8df/Untitled.png](03%2016%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%E1%84%8B%E1%85%AA%20Cost%20Function%20484740d78af3487f8c9958ef2aabb8df/Untitled.png)

![03%2016%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%E1%84%8B%E1%85%AA%20Cost%20Function%20484740d78af3487f8c9958ef2aabb8df/Untitled%203.png](03%2016%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%E1%84%8B%E1%85%AA%20Cost%20Function%20484740d78af3487f8c9958ef2aabb8df/Untitled%203.png)