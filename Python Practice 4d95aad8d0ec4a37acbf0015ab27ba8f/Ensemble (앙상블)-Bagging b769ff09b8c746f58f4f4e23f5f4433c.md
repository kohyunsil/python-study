# Ensemble (앙상블)-Bagging

데이터 로드

```python
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
```

```python
url = 'https://raw.githubusercontent.com/PinkWink/ML_tutorial/master/dataset/HAR_dataset/features.txt'
feature_name_df = pd.read_csv(url, sep='\s+', header=None,
                             names=['column_index', 'column_name'])
feature_name_df.head()
```

```python
feature_name = feature_name_df.iloc[:, 1].values.tolist()
feature_name[:10]
```

![Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled.png](Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled.png)

x 데이터

```python
X_train_url = 'https://raw.githubusercontent.com/PinkWink/ML_tutorial/master/dataset/HAR_dataset/train/X_train.txt'
X_test_url = 'https://raw.githubusercontent.com/PinkWink/ML_tutorial/master/dataset/HAR_dataset/test/X_test.txt'

X_train = pd.read_csv(X_train_url, sep='\s+', header=None)
X_test = pd.read_csv(X_test_url, sep='\s+', header=None)
```

```python
X_train.columns = feature_name
X_test.columns = feature_name
X_train.head()
```

![Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%201.png](Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%201.png)

 y 데이터 읽기

```python
y_train_url = 'https://raw.githubusercontent.com/PinkWink/ML_tutorial/master/dataset/HAR_dataset/train/y_train.txt'
y_test_url = 'https://raw.githubusercontent.com/PinkWink/ML_tutorial/master/dataset/HAR_dataset/test/y_test.txt'

y_train = pd.read_csv(y_train_url, sep='\s+', header=None, names=['action'])
y_test = pd.read_csv(y_test_url, sep='\s+', header=None, names=['action'])
```

각 액션별 데이터의 수 찾기

```python
X_train.shape, X_test.shape, y_train.shape, y_test.shape
```

![Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%202.png](Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%202.png)

```python
y_train['action'].value_counts()
```

![Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%203.png](Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%203.png)

결정나무

```python
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score

dt_clf = DecisionTreeClassifier(random_state=13, max_depth=4)
dt_clf.fit(X_train, y_train)
pred = dt_clf.predict(X_test)

accuracy_score(y_test, pred)
```

GridSearchCV (max_depth 다양하게 설정)

```python
from sklearn.model_selection import GridSearchCV

params = {
    'max_depth' : [4, 6, 8, 10, 12, 16, 20, 24]
}

grid_cv = GridSearchCV(dt_clf, param_grid=params, scoring='accuracy',
                      cv=5, return_train_score=True)
grid_cv.fit(X_train, y_train)
```

적합한 max_depth 찾기

```python
grid_cv.best_score_, grid_cv.best_params_
```

MAX_depth 별 성능 정리

```python
cv_results_df = pd.DataFrame(grid_cv.cv_results_)
cv_results_df[['param_max_depth', 'mean_test_score', 'mean_train_score']]
```

![Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%204.png](Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%204.png)

실제 test 데이터에서의 결과

```python
max_depths = [6, 8, 10, 12, 16, 20, 24]

for depth in max_depths:
    dt_clf = DecisionTreeClassifier(max_depth=depth, random_state=156)
    dt_clf.fit(X_train, y_train)
    pred = dt_clf.predict(X_test)
    accuracy = accuracy_score(y_test, pred)
    print('Max_Depth =', depth, ', Accuracy = ', accuracy)
```

![Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%205.png](Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%205.png)

Best model 의 결과는

```python
best_df_clf = grid_cv.best_estimator_
pred1 = best_df_clf.predict(X_test)

accuracy_score(y_test, pred1)
```

![Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%206.png](Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%206.png)

RandomForestClassifier

```python
from sklearn.model_selection import GridSearchCV
from sklearn.ensemble import RandomForestClassifier

params = {
    
    'max_depth' : [6, 8, 10],
    'n_estimators' : [50, 100, 200],
    'min_samples_leaf' : [8, 12],
    'min_samples_split' : [8, 12]
}

rf_clf = RandomForestClassifier(random_state=13, n_jobs=-1)
grid_cv = GridSearchCV(rf_clf, param_grid=params, cv=2, n_jobs=-1)
grid_cv.fit(X_train, y_train)
```

```python
cv_results_df = pd.DataFrame(grid_cv.cv_results_)
cv_results_df.columns
```

성능이 좋다

```python
target_col = ['rank_test_score', 'mean_test_score', 'param_n_estimators',
             'param_max_depth']
cv_results_df[target_col].sort_values('rank_test_score').head()
```

![Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%207.png](Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%207.png)

Best model

```python
grid_cv.best_params_, grid_cv.best_score_
```

![Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%208.png](Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%208.png)

Test 데이터에 적용

```python
rf_clf_best = grid_cv.best_estimator_
rf_clf_best.fit(X_train, y_train)

pred1 = rf_clf_best.predict(X_test)

accuracy_score(y_test, pred1)
```

![Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%209.png](Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%209.png)

각 특성들의 중요도가 개별적으로 높지 않다

![Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%2010.png](Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%2010.png)

```python
best_cols_values = rf_clf_best.feature_importances_
best_cols = pd.Series(best_cols_values, index=X_train.columns)
top20_cols = best_cols.sort_values(ascending=False)[:20]
top20_cols
```

![Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%2011.png](Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%2011.png)

주요 특성 관찰

```python
import seaborn as sns

plt.figure(figsize=(8,8))
sns.barplot(x=top20_cols, y=top20_cols.index)
plt.show(
```

![Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%2012.png](Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%2012.png)

20개 특성만 가지고 성능 확인

```python
top20_cols.index

X_train_re = X_train[top20_cols.index]
X_test_re = X_test[top20_cols.index]

rf_clf_best_re = grid_cv.best_estimator_
rf_clf_best_re.fit(X_train_re, y_train.values.reshape(-1,))

pred1_re = rf_clf_best_re.predict(X_test_re)
accuracy_score(y_test, pred1_re)
```

![Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%2013.png](Ensemble%20(%E1%84%8B%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%87%E1%85%B3%E1%86%AF)-Bagging%20b769ff09b8c746f58f4f4e23f5f4433c/Untitled%2013.png)