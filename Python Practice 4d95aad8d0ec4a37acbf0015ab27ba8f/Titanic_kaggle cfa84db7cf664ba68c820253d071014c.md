# Titanic_kaggle

타이타닉 데이터 분류모델 만들기

- 캐글에 업로드
- 전처리 데이터와 전처리 하지 않은 데이터의 결과 비교
- 5가지 모델 알고리즘을 이용해서 결과를 확인

데이터 로드

```python
titanic_df = pd.read_csv("datas/train.csv")
titanic_df.tail(1)
```

데이터 전처리

```python
# 전처리 1 : 수치형 데이터 컬럼만 사용

columns = ["Pclass", "Age", "SibSp", "Parch", "Fare", "Survived"]
titanic_1 = titanic_df[columns]
titanic_1.dropna(inplace=True)
titanic_1.reset_index(drop=True, inplace=True)
titanic_1.tail(2)
```

![Titanic_kaggle%20cfa84db7cf664ba68c820253d071014c/Untitled.png](Titanic_kaggle%20cfa84db7cf664ba68c820253d071014c/Untitled.png)

전처리 2 : 더미변수화 ( 범주형 데이터를 더미변수화해서 사용)

```python
columns = ["Pclass", "Age", "SibSp", "Parch", "Fare", "Sex", "Embarked", "Survived"]
titanic_2 = titanic_df[columns]

# 더미 변수화
dummy_1 = pd.get_dummies(titanic_2["Sex"])
dummy_2 = pd.get_dummies(titanic_2["Embarked"])

# 더미 변수화한 데이터 프레임 합치기
titanic_2 = pd.concat([titanic_2, dummy_1, dummy_2], axis=1)

# NaN 데이터가 있는 row 삭제
titanic_2.dropna(inplace=True)

# reset index
titanic_2.reset_index(drop=True, inplace=True)

# 범주형 데이터가 있는 Sex, Embarked 컬럼 제거
titanic_2.drop(columns=["Sex", "Embarked"], inplace=True)
titanic_2.tail(2)
```

데이터셋 분리

```python
from sklearn.model_selection import train_test_split

df_x = titanic_1.drop(columns=["Survived"])
df_y = titanic_1["Survived"]

# df_x = titanic_2.drop(columns=["Survived"])
# df_y = titanic_2["Survived"]

train_x, test_x, train_y, test_y = train_test_split(df_x, df_y, test_size=0.2,
                                                    random_state=1)
train_x.shape, test_x.shape, train_y.shape, test_y.shape
```

5가지 모델 학습

```python
# 1. DecisionTree
from sklearn.tree import DecisionTreeClassifier

dt_model = DecisionTreeClassifier(random_state=0).fit(train_x, train_y)
dt_acc = np.round(dt_model.score(test_x, test_y) * 100, 2) 
dt_acc
```

```python
# 2. RandomForest
from sklearn.ensemble import RandomForestClassifier

rf_model = RandomForestClassifier(n_estimators=10, random_state=0).fit(train_x, train_y)
rf_acc = np.round(rf_model.score(test_x, test_y) * 100, 2) 
rf_acc
```

```python
# 3. Gaussian Naive Bayes
from sklearn.naive_bayes import GaussianNB

gnb_model = GaussianNB().fit(train_x, train_y)
gnb_acc = np.round(gnb_model.score(test_x, test_y) * 100, 2) 
gnb_acc
```

```python
# 4. Logistic Regression
from sklearn.linear_model import LogisticRegression
lr_model = LogisticRegression().fit(train_x, train_y)
lr_acc = np.round(lr_model.score(test_x, test_y) * 100, 2) 
lr_acc
```

```python
# 5. support vector machine
from sklearn.svm import SVC

svc_model = SVC().fit(train_x, train_y)
svc_acc = np.round(svc_model.score(test_x, test_y) * 100, 2) 
svc_acc
```

전처리 결과

![Titanic_kaggle%20cfa84db7cf664ba68c820253d071014c/Untitled%201.png](Titanic_kaggle%20cfa84db7cf664ba68c820253d071014c/Untitled%201.png)

제출할 데이터 파일 만들기

```python
rf_model
```

![Titanic_kaggle%20cfa84db7cf664ba68c820253d071014c/Untitled%202.png](Titanic_kaggle%20cfa84db7cf664ba68c820253d071014c/Untitled%202.png)

```python
# 제출할 파일
submission_df = pd.read_csv("datas/gender_submission.csv")
submission_df.tail(2)
```

```python
# 테스트 파일 불러오기
test_df = pd.read_csv("datas/test.csv")

# 테스트 파일 전처리
columns = ["Pclass", "Age", "SibSp", "Parch", "Fare", "Sex", "Embarked"]
test_df = test_df[columns]

# 더미 변수화
dummy_1 = pd.get_dummies(test_df["Sex"])
dummy_2 = pd.get_dummies(test_df["Embarked"])

# 더미 변수화한 데이터 프레임 합치기
test_df = pd.concat([test_df, dummy_1, dummy_2], axis=1)

# reset index
test_df.reset_index(drop=True, inplace=True)

# 범주형 데이터가 있는 Sex, Embarked 컬럼 제거
test_df.drop(columns=["Sex", "Embarked"], inplace=True)
test_df.tail(2)
```

```python
import missingno as msno
msno.matrix(test_df)
plt.show()
```

![Titanic_kaggle%20cfa84db7cf664ba68c820253d071014c/Untitled%203.png](Titanic_kaggle%20cfa84db7cf664ba68c820253d071014c/Untitled%203.png)

```python
%matplotlib inline
%config InlineBackend.figure_formats = {'png', 'retina'}
```

```python
import matplotlib as mpl
import matplotlib.pyplot as plt
```

```python
sns.distplot(titanic_df.Fare)
plt.show()
```

![Titanic_kaggle%20cfa84db7cf664ba68c820253d071014c/Untitled%204.png](Titanic_kaggle%20cfa84db7cf664ba68c820253d071014c/Untitled%204.png)

Fare 의 NaN 데이터 중앙값으로 대체

```python
test_df["Fare"][test_df.Fare.isnull()] = np.median(test_df.Fare.dropna())

msno.matrix(test_df)
plt.show()
```

![Titanic_kaggle%20cfa84db7cf664ba68c820253d071014c/Untitled%205.png](Titanic_kaggle%20cfa84db7cf664ba68c820253d071014c/Untitled%205.png)

```python
sns.distplot(titanic_df.Age)
plt.show()
```

![Titanic_kaggle%20cfa84db7cf664ba68c820253d071014c/Untitled%206.png](Titanic_kaggle%20cfa84db7cf664ba68c820253d071014c/Untitled%206.png)

```python
test_df["Age"][test_df.Age.isnull()] = np.median(test_df.Age.dropna())

msno.matrix(test_df)
plt.show()
```

![Titanic_kaggle%20cfa84db7cf664ba68c820253d071014c/Untitled%207.png](Titanic_kaggle%20cfa84db7cf664ba68c820253d071014c/Untitled%207.png)