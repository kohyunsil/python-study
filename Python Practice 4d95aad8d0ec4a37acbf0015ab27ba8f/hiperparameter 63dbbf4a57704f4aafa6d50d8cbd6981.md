# hiperparameter

하이퍼파라미터 튜닝

- 모델의 성능을 확보하기 위해 조절하는 설정값
- 튜닝 대상 :  max_depth 를 바ㅏ꿔가며 테스트 가능

데이터 로드

```python
import pandas as pd
red_url = 'https://raw.githubusercontent.com/PinkWink/ML_tutorial/master/dataset/winequality-red.csv'
white_url = 'https://raw.githubusercontent.com/PinkWink/ML_tutorial/master/dataset/winequality-white.csv'

red_wine = pd.read_csv(red_url, sep=';')
white_wine = pd.read_csv(white_url, sep=';')

red_wine['color'] = 1.
white_wine['color'] = 0.

wine = pd.concat([red_wine, white_wine])
wine['taste'] = [1. if grade>5 else 0. for grade in wine['quality']]

X = wine.drop(['taste', 'quality'], axis=1)
y = wine['taste']
```

gridSearch CV

- 결과를 확인하고 싶은 파라미터를 정의
- cv 는 cross validation
- n_jobs 옵션을 높여주면 CPU의 코어를 보다 병렬로 활용

```python
from sklearn.model_selection import GridSearchCV
from sklearn.tree import DecisionTreeClassifier

params = {'max_depth' : [2, 4, 7, 10]}
wine_tree = DecisionTreeClassifier(max_depth=2, random_state=13)

gridsearch = GridSearchCV(estimator=wine_tree, param_grid=params, cv=5)
gridsearch.fit(X, y)
```

![hiperparameter%2063dbbf4a57704f4aafa6d50d8cbd6981/Untitled.png](hiperparameter%2063dbbf4a57704f4aafa6d50d8cbd6981/Untitled.png)

gridSearch CV의 결과

```python
import pprint

pp = pprint.PrettyPrinter(indent=4)
pp.pprint(gridsearch.cv_results_)
```

![hiperparameter%2063dbbf4a57704f4aafa6d50d8cbd6981/Untitled%201.png](hiperparameter%2063dbbf4a57704f4aafa6d50d8cbd6981/Untitled%201.png)

최적의 성능을 가진 모델 찾기

```python
gridsearch.best_estimator_

#####
gridsearch.best_score_

##### max_depth : 2
gridsearch.best_params_
```

pipeline 을 적용한 모델에 GridSearch 적용

```python
from sklearn.pipeline import Pipeline
from sklearn.tree import DecisionTreeClassifier
from sklearn.preprocessing import StandardScaler

estimators = [('scaler', StandardScaler()),
              ('clf', DecisionTreeClassifier(random_state=13))]

pipe = Pipeline(estimators)
```

```python
param_grid = [ {'clf__max_depth':[2, 4, 7, 10]} ]

GridSearch = GridSearchCV(estimator=pipe, param_grid=param_grid, cv=5)
GridSearch.fit(X, y)
```

best model

```python
GridSearch.best_estimator_

GridSearch.best_score_

GridSearch.cv_results_
```

```python
# 표로 성능 확인
import pandas as pd

score_df = pd.DataFrame(GridSearch.cv_results_)
score_df[['params', 'rank_test_score', 'mean_test_score', 'std_test_score']]
```

![hiperparameter%2063dbbf4a57704f4aafa6d50d8cbd6981/Untitled%202.png](hiperparameter%2063dbbf4a57704f4aafa6d50d8cbd6981/Untitled%202.png)