# 03/17 Preprocessing

- 데이터 분석이나 모델링을 하기 전에 전처리하는 방법들을 학습

결측 데이터 검색

- missing 패키지 : 시각화

```python
!pip install missingno
```

EX 타이타닉 승객 데이터 결측치 전처리

```python
import seaborn as sns

titanic_df = sns.load_dataset("titanic")
titanic_df.tail(2)
```

```python
msno.matrix(titanic_df)
plt.show()
```

![03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled.png](03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled.png)

데이터가 20% 이상이 없으면 컬럼 삭제 > deck 컬럼 삭제

```python
result_df = titanic_df.dropna(thresh=int(len(titanic_df) * 0.8), axis=1)
msno.matrix(result_df)
plt.show()
```

![03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%201.png](03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%201.png)

갯수확인

```python
result_df.info()
```

![03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%202.png](03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%202.png)

결측 데이터의 처리 2 : 대체

- 수치형 : 비결측 데이터의 평균, 중앙값으로 대체 : -1, 0, 1로도 대체 가능
- 범주형 : 최빈값으로 대체

```python
result_df.tail(2)
```

```python
# 최빈값으로 채우기
from sklearn.impute import SimpleImputer

imputer = SimpleImputer(strategy="most_frequent")
```

```python
sns.countplot(result_df.embarked)
plt.show()
```

![03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%203.png](03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%203.png)

```python
result_df["embarked"] = imputer.fit_transform(result_df[["embarked"]])
result_df["embark_town"] = imputer.fit_transform(result_df[["embark_town"]])
```

```python
msno.matrix(result_df)
plt.show()
```

![03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%204.png](03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%204.png)

수치형 데이터 채우기

- 평균 : 정규분포가 대칭으로 나오면 사용
- 중앙값 : 정규분포가 비대칭으로 나오면 사용

```python
sns.distplot(result_df.age)
plt.show()
```

![03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%205.png](03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%205.png)

age 컬럼의 결측치 데이터를 중앙값으로 채움

```python
imputer = SimpleImputer(strategy="median")
np.median(result_df["age"].dropna())  # 중앙값

imputer.fit_transform(result_df[["age"]])
```

![03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%206.png](03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%206.png)

스케일링, 변환

- StandardScaler
    - 평균 0, 표준편차가 1이 되도록 스케일링
- RobustScaler
    - 중앙값이 0이 되도록 스케일링
    - 아웃라이어가 있어도 데이터가 한쪽으로 쏠림을 보완해줌

🥑 스탠다드스케일러 하기 전

```python
data1 = np.arange(7).reshape(-1, 1)
data1
```

![03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%207.png](03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%207.png)

```python
np.mean(data1), np.std(data1)
```

out  : (3.0, 2.0)

🥑 StandardScaler (평균 0, 표편 1)

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

data2 = scaler.fit_transform(data1)
data2
```

![03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%208.png](03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%208.png)

- 평균은 0으로 표준편차는 1로 스케일을 줄여줌으로써 이상값(outlier)의 영향을 최대한 줄여줌

🥑 RobustScaler (중앙값 0)

![03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%209.png](03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%209.png)

```python
# RobustScaler

from sklearn.preprocessing import RobustScaler

scaler = RobustScaler()

data = scaler.fit_transform(data3)
data
```

![03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%2010.png](03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%2010.png)

범주형 데이터의 수치화

- 더미 변수화
    - full-rank : 모든 카테고리를 컬럼으로 설정하여 수치화
    - reduce-rank : 하나의 카테고리를 기준으로 나머지 컬럼을 수치화
- 카테고리 임베딩

```python
datas = np.random.choice(["A", "B", "O", "AB"], 10)
df = pd.DataFrame({"blood": datas})
df.tail(3)
```

full-rank

```python
fr_df = pd.get_dummies(df)
fr_df.tail(3)
```

![03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%2011.png](03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%2011.png)

reduce rank

```python
fr_df["blood_A"] = 1 # 기준을 하나 잡는다
fr_df.tail(3)
```

![03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%2012.png](03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%2012.png)

두개 이상의 범주형 변수에 대한 더미 변수화

```python
datas_1 = np.random.choice(["A", "B", "O", "AB"], 100)
datas_2 = np.random.choice(["male", "female"], 100)
df = pd.DataFrame({"blood": datas_1, "sex": datas_2})
df.tail(3)
```

![03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%2013.png](03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%2013.png)

통합축소형 방식

```python
result_df = pd.get_dummies(df)
result_df["intercept"] = 1
result_df.tail(3)
```

![03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%2014.png](03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%2014.png)

상호작용 방식

```python
result_df = pd.get_dummies(df["blood"] + "_" + df["sex"])
result_df.tail(3)
```

![03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%2015.png](03%2017%20Preprocessing%209b9dbe2b5d334c95a17cca9b9f1e6280/Untitled%2015.png)

카테고리 임베딩

- 카테고리 데이터를 대체할 수 있는 수치형 데이터로 변환
- 컬럼 : 서울, 부산, 인천 > 1000, 300, 200