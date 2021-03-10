# 03/10 Pandas_series

pandas_series

- 데이터 분석을 위한 사용성이 좋고, 쉽고 성능에 좋은 오픈소스 Python 라이브러리
- R의 DataFrame 구조로 만들어짐
    - R보다 pandas가 성능 좋음 : numpy로 동작
    - Python이 더 범용 개발에 사용
    - R보다 Python이 쉽다.
- 두 가지 데이터 타입 중 Series
    - Series
        - index, value로 이루어진 데이터 타입
        - 하나의 Series에는 동일한 데이터 타입의 데이터만 사용

```python
# 선언

s = pd.Series([1, 3, 5, np.nan, 6, 8]) # nan = null, None

data = pd.Series(np.random.randint(10, size=5))
data
```

```python
# index 설정

data = pd.Series(np.random.randint(10, size=5), list("ABCDE"))

```

![03%2010%20Pandas_series%20fd707da28dd6468ab2b589f06d5c2e60/_2021-03-10__5.40.56.png](03%2010%20Pandas_series%20fd707da28dd6468ab2b589f06d5c2e60/_2021-03-10__5.40.56.png)

 series 객체의 값 확인 : index, value

```python
data.index, data.values
```

[] : 마스킹 문법 사용

```python
data["C"], data.C, data[["C", "E"]]

# s.1 처럼 숫자가 가장 앞에 올수 x
s[1] # 이렇게 작성해야함.
```

![03%2010%20Pandas_series%20fd707da28dd6468ab2b589f06d5c2e60/_2021-03-10__5.44.04.png](03%2010%20Pandas_series%20fd707da28dd6468ab2b589f06d5c2e60/_2021-03-10__5.44.04.png)

Series 객체에 Name, index_name을 설정

```python
data.name = "Data"
data.index.name = "ABC"
```

 series 객체의 연산

```python
data * 10 # 브로드캐스팅되게 연산 수행됨

data[["A", "C"]], data[2::-1]

data > 3, data[data > 3] # 비교연산자도 numpy와 사용법 비슷
```

```python
#### items()
list(data.items())

#### items()
for idx, val in data.items():
	  print(idx,val)
```

![03%2010%20Pandas_series%20fd707da28dd6468ab2b589f06d5c2e60/_2021-03-10__5.49.50.png](03%2010%20Pandas_series%20fd707da28dd6468ab2b589f06d5c2e60/_2021-03-10__5.49.50.png)

 데이터 수정

```python
data["B"] = 10

data[["B", "C"]] = [100, 200]

data[data >= 100] = 5
```

Series 객체와 Series 객체의 연산

- index가 같은 데이터끼리 연산

```python
data1 = pd.Series(np.random.randint(10, size=5), list("ABCDE"))
data2 = pd.Series({"D":3, "E":5, "F":7})

result = data1 + data2
result
```

![03%2010%20Pandas_series%20fd707da28dd6468ab2b589f06d5c2e60/_2021-03-10__5.52.59.png](03%2010%20Pandas_series%20fd707da28dd6468ab2b589f06d5c2e60/_2021-03-10__5.52.59.png)

```python
# 위의 경우 계산을 하기 위해서는 null값을 적용해준 뒤 할당시켜준다.
result[result.isnull()] = data1
result[result.isnull()] = data2
result
```

![03%2010%20Pandas_series%20fd707da28dd6468ab2b589f06d5c2e60/_2021-03-10__5.55.07.png](03%2010%20Pandas_series%20fd707da28dd6468ab2b589f06d5c2e60/_2021-03-10__5.55.07.png)

```python
# 데이터 형변환
result.dtype

result = result.astype(np.int)
result.dtype
```