# 03/10 Pandas_DataFrame

DataFrame

- index, value, column으로 이루어진 데이터 타입
- 여러개의 Series(column)로 이루어진 데이터타입
- 테이블 형태의 데이터
- 

### 1. 데이터 프레임 객체 생성

- 리스트 안에 딕셔너리로 생성
    - `[{},{},{}]` : `{column:value, ...}` > row
- 딕셔너리 안에 리스트로 생성 :
    - `{column1: [values], column2: [values]}` : `column1:[values]` > column

```python
# 리스트의 딕셔너리

datas = [
		{"name": "data", "email": "data@gmail.com", "id":1},
		{"name": "python", "email": "python@naver.com", "id": 2},
]
datas

df = pd.DataFrame(datas)
df
```

```python
# 딕셔너리의 리스트

datas = df.to_dict("list")
datas

```

 데이터 프레임의 변수 확인 : index, values, columns

```python
datas = {'name': ['data', 'python', "jupyter"],
         'email': ['data@gmail.com', 'python@naver.com', 'jupyter@daum.net'],
         'id': [1, 2, 3]}

df = pd.DataFrame(datas)
df

##### 변수확인
df.index, df.columns, df.values
#####
df.index = list("ABC")
df
#####
df.dtypes
```

![03%2010%20Pandas_DataFrame%20a098f3edcc1e4105a80c9c79da5ba7ee/_2021-03-10__6.06.56.png](03%2010%20Pandas_DataFrame%20a098f3edcc1e4105a80c9c79da5ba7ee/_2021-03-10__6.06.56.png)

데이터의 선택

```python
# 데이터 선택 : row, column, [row, column]

df.index = [0, 1, 2]
df
```

```python
# row : df.loc[]
df.loc[::-1]
```

![03%2010%20Pandas_DataFrame%20a098f3edcc1e4105a80c9c79da5ba7ee/Untitled.png](03%2010%20Pandas_DataFrame%20a098f3edcc1e4105a80c9c79da5ba7ee/Untitled.png)

```python
df.loc[[0, 2]]
```

![03%2010%20Pandas_DataFrame%20a098f3edcc1e4105a80c9c79da5ba7ee/Untitled%201.png](03%2010%20Pandas_DataFrame%20a098f3edcc1e4105a80c9c79da5ba7ee/Untitled%201.png)

```python
# column : []

df[["id", "name", "email"]] #순서바꾸기 가능
```

![03%2010%20Pandas_DataFrame%20a098f3edcc1e4105a80c9c79da5ba7ee/Untitled%202.png](03%2010%20Pandas_DataFrame%20a098f3edcc1e4105a80c9c79da5ba7ee/Untitled%202.png)

```python
# [row, column] : df.loc[row, column]

df.loc[::2, ["id", "email"]] # 2칸씩 뛰기 + id랑 이메일만출력
```

![03%2010%20Pandas_DataFrame%20a098f3edcc1e4105a80c9c79da5ba7ee/Untitled%203.png](03%2010%20Pandas_DataFrame%20a098f3edcc1e4105a80c9c79da5ba7ee/Untitled%203.png)

데이터의 수정  =  데이터 선택  :  수정하는 데이터

```python
df.loc[1:, ["name", "id"]] = [["notebook", 7], ["macbook", 8]]
df
```

![03%2010%20Pandas_DataFrame%20a098f3edcc1e4105a80c9c79da5ba7ee/Untitled%204.png](03%2010%20Pandas_DataFrame%20a098f3edcc1e4105a80c9c79da5ba7ee/Untitled%204.png)

데이터의 추가 : column, row

- row (??)

```python
len(df)

df.loc[len(df)] = ["iphone", "iphone@daum.net", 9]
df
```

![03%2010%20Pandas_DataFrame%20a098f3edcc1e4105a80c9c79da5ba7ee/Untitled%205.png](03%2010%20Pandas_DataFrame%20a098f3edcc1e4105a80c9c79da5ba7ee/Untitled%205.png)

- column

```python
df["addr"] = ["seoul", "pusan", "incheon", "anyang"]
df
```

![03%2010%20Pandas_DataFrame%20a098f3edcc1e4105a80c9c79da5ba7ee/Untitled%206.png](03%2010%20Pandas_DataFrame%20a098f3edcc1e4105a80c9c79da5ba7ee/Untitled%206.png)

데이터의 삭제 : df.drop()

```python
df.drop(index=[3], columns=["addr"])
```

df.iloc[]

```python

df.loc[:-1], df.iloc[:-1]
```

![03%2010%20Pandas_DataFrame%20a098f3edcc1e4105a80c9c79da5ba7ee/Untitled%207.png](03%2010%20Pandas_DataFrame%20a098f3edcc1e4105a80c9c79da5ba7ee/Untitled%207.png)