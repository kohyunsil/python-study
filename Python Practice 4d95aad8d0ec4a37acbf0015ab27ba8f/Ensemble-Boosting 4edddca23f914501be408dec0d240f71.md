# Ensemble-Boosting

- 배깅 - 데이터 전체를 학습
- 부스팅 - 학습시키고 틀린것만 재학습

데이터 로드 ( 와인데이터 )

```python
import pandas as pd

wine_url = 'https://raw.githubusercontent.com/PinkWink/ML_tutorial/master/dataset/wine.csv'

wine = pd.read_csv(wine_url, index_col=0)
wine.head()
```

```python
wine['taste'] = [1. if grade>5 else 0. for grade in wine['quality']]

X = wine.drop(['taste', 'quality'], axis=1)
y = wine['taste']
```

pipeline이 아니라 직접 StandardScaler 적용

```python
from sklearn.preprocessing import StandardScaler

sc = StandardScaler()
X_sc = sc.fit_transform(X)
```

- 데이터 나누기

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X_sc, y, test_size=0.2, random_state=13)
```

```python
import matplotlib.pyplot as plt
%matplotlib inline

wine.hist(bins=10, figsize=(15,10))
plt.show()
```

![Ensemble-Boosting%204edddca23f914501be408dec0d240f71/Untitled.png](Ensemble-Boosting%204edddca23f914501be408dec0d240f71/Untitled.png)

 quality 별 다른 특성이 어떤지 확인

```python
colum_names = ['fixed acidity', 'volatile acidity', 'citric acid',
              'residual sugar', 'chlorides', 'free sulfur dioxide',
              'total sulfur dioxide', 'density', 'pH', 'sulphates', 'alcohol']
df_pivot_table = wine.pivot_table(colum_names, ['quality'], aggfunc='median')
print(df_pivot_table)
```

quality 에 대한 나머지 특성들의 상관관계

```python
corr_matrix = wine.corr()
print(corr_matrix['quality'].sort_values(ascending=False))
```

tast 컬럼의 분포

```python
import seaborn as sns

sns.countplot(wine['taste'])
plt.show()
```

![Ensemble-Boosting%204edddca23f914501be408dec0d240f71/Untitled%201.png](Ensemble-Boosting%204edddca23f914501be408dec0d240f71/Untitled%201.png)

한 번에 모델 테스트

```python
from sklearn.ensemble import (AdaBoostClassifier, GradientBoostingClassifier,
                             RandomForestClassifier)
from sklearn.tree import DecisionTreeClassifier
from sklearn.linear_model import LogisticRegression

models = []
models.append(('RandomForestClassifier', RandomForestClassifier()))
models.append(('DecisionTreeClassifier', DecisionTreeClassifier()))
models.append(('AdaBoostClassifier', AdaBoostClassifier()))
models.append(('GradientBoostingClassifier', GradientBoostingClassifier()))
models.append(('LogisticRegression', LogisticRegression()))
```

결과 저장

```python
from sklearn.model_selection import KFold, cross_val_score, KFold

results = []
names = []

for name, model in models:
    kfold = KFold(n_splits=5, random_state=13, shuffle=True)
    cv_results = cross_val_score(model, X_train, y_train,
                                cv=kfold, scoring='accuracy')
    results.append(cv_results)
    names.append(name)
    
    print(name, cv_results.mean(), cv_results.std())
```

![Ensemble-Boosting%204edddca23f914501be408dec0d240f71/Untitled%202.png](Ensemble-Boosting%204edddca23f914501be408dec0d240f71/Untitled%202.png)

cross-validation 결과 확인 

```python
fig = plt.figure(figsize=(14, 8))
fig.suptitle('Algorithm Comparison')
ax = fig.add_subplot(111)
plt.boxplot(results)
ax.set_xticklabels(names)
plt.show()
```

![Ensemble-Boosting%204edddca23f914501be408dec0d240f71/Untitled%203.png](Ensemble-Boosting%204edddca23f914501be408dec0d240f71/Untitled%203.png)

테스트 데이터에 대한 평가 결과

```python
from sklearn.metrics import accuracy_score

for name, model in models:
    model.fit(X_train, y_train)
    pred = model.predict(X_test)
    print(name, accuracy_score(y_test, pred))
```

![Ensemble-Boosting%204edddca23f914501be408dec0d240f71/Untitled%204.png](Ensemble-Boosting%204edddca23f914501be408dec0d240f71/Untitled%204.png)