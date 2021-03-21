# Dask

- 데이터 분석을 수행할때 큰 파일의 csv파일은 메모리상에 모두 올리기 어렵다
- 가상의 데이터 프레임을 병렬처리를 활용해서 메모리를 효율적으로 사용할수 있도록 도와주는 패키지
- 가상의 데이터 프레임, 병렬처리 작업 스케쥴러
- 설치: $ conda install dask

```python
!conda install dask

import dask.dataframe as dd
```

파일 읽기

```python
# 데이터 프레임으로 읽어오기
df = pd.read_csv("datas/Crimes_-_2001_to_Present.csv")

# Dask의 데이터 프레임으로 읽어오기
ddf = dd.read_csv("datas/Crimes_-_2001_to_Present.csv", dtype=str)

type(ddf)

ddf.tail(2)
```

파일 저장하기

```python
!mkdir datas/crimes

ddf.to_csv("datas/crimes/data_*.csv", index=False)
```

저장한 파일 불러오기

```python
load_df = dd.read_csv("datas/crimes/data_*.csv")
load_df.tail(2)
```

![Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled.png](Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled.png)

객체의 함수 실행하기

```python
%%time
seoul_df = pd.read_csv("datas/seoul.csv", dtype={"type_of_business": np.str})

seoul_df.tail(1)
```

![Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%201.png](Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%201.png)

```python
%%time
df_1 = seoul_df.drop(columns=["Unnamed: 0"])

df_1.tail(1)
```

![Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%202.png](Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%202.png)

Dask

```python
%%time
seoul_df = dd.read_csv("datas/seoul.csv", dtype={"type_of_business": np.str})

%%time
df_2 = seoul_df.drop(columns=["Unnamed: 0"])
```

연산수행 : compute()

```python
%%time
df_1.amount.mean()

%%time
df_2.amount.mean().compute()
```

![Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%203.png](Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%203.png)

dask의 데이터 프레임을 pandas의 데이터 프레임으로 변환

```python
df_2.compute().tail(2)
```

![Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%204.png](Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%204.png)

assign() : 연산한 결과를 바로 컬럼에 할당하는 함수

```python
df_1["amount_2"] = df_1.amount * 100
df_1.tail(2)
```

![Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%205.png](Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%205.png)

```python
df_2.assign(amount_2 = df_2.amount * 100).tail(2)
```

![Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%206.png](Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%206.png)

병렬처리

- cpu 코어 수만큼 병렬처리가 가능

```python
ddf.head(2)
```

![Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%207.png](Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%207.png)

```python
%%time
result_df = ddf.dropna().compute()
```

```python
len(result_df), len(ddf)
```

![Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%208.png](Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%208.png)

프로그래스바 설정

```python
from dask.diagnostics import ProgressBar
ProgressBar().register()
```

컬럼별 카운트

```python
ddf.count().compute()
```

![Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%209.png](Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%209.png)

병렬처리 시작 : cpu 코어 수 확인

```python
import os
os.cpu_count()
```

![Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%2010.png](Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%2010.png)

```python
ddf.count().compute(scheduler="processes", num_workers=4)
```

![Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%2011.png](Dask%206b01b2959dff4debb74b1df6bdf8ae62/Untitled%2011.png)