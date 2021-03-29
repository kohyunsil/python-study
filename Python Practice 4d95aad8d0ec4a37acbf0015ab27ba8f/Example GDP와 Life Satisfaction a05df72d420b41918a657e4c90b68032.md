# Example : GDP와 Life Satisfaction

데이터 로드, 정리

```python
import sklearn
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import sklearn.linear_model

%matplotlib inline
import matplotlib as mpl
mpl.rc('axes', labelsize=14)
mpl.rc('xtick', labelsize=12)
mpl.rc('ytick', labelsize=12)
```

```python
oecd_bli = pd.read_csv("./datas/oecd_bli_2015.csv", thousands=',')
gdp_per_capita = pd.read_csv("./datas/gdp_per_capita.csv",
                            thousands=',',
                            delimiter='\t',
                            encoding='latin1', na_values="n/a")
oecd_bli.head()
```

```python
#데이터정리
oecd_bli = oecd_bli[oecd_bli["INEQUALITY"]=="TOT"]
oecd_bli.head()

#국가별로정리
oecd_bli = oecd_bli.pivot(index="Country", columns="Indicator", values="Value")
oecd_bli.head()
```

데이터 결합

```python
#데이터결합
gdp_per_capita.rename(columns={"2015": "GDP per capita"}, inplace=True)
gdp_per_capita.set_index("Country", inplace=True)
full_country_stats = pd.merge(left=oecd_bli, right=gdp_per_capita,
                             left_index=True, right_index=True)
full_country_stats.head()

# 정렬
full_country_stats.sort_values(by="GDP per capita", inplace=True)
full_country_stats.head()
```

이상치 정리

```python
# 이상치 정리
remove_indices = [0, 1, 6, 8, 33, 34, 35]
keep_indices = list(set(range(36)) - set(remove_indices))
full_country_stats[["GDP per capita",
                   'Life satisfaction']].iloc[keep_indices]
full_country_stats.head()
```

![Example%20GDP%E1%84%8B%E1%85%AA%20Life%20Satisfaction%20a05df72d420b41918a657e4c90b68032/Untitled.png](Example%20GDP%E1%84%8B%E1%85%AA%20Life%20Satisfaction%20a05df72d420b41918a657e4c90b68032/Untitled.png)

데이터 정리

```python
# 다시 데이터 로드
oecd_bli = pd.read_csv("./datas/oecd_bli_2015.csv", thousands=',')
gdp_per_capita = pd.read_csv("./datas/gdp_per_capita.csv",
                            thousands=',',
                            delimiter='\t',
                            encoding='latin1', na_values="n/a")
```

함수로 정리

```python
def prepare_country_stats(oecd_bli, gdp_per_capita):
    oecd_bli = oecd_bli[oecd_bli["INEQUALITY"]=="TOT"]
    oecd_bli = oecd_bli.pivot(index="Country", columns="Indicator",
                             values="Value")
    gdp_per_capita.rename(columns={"2015" : "GDP per capita"}, inplace=True)
    gdp_per_capita.set_index("Country", inplace=True)
    full_country_stats = pd.merge(left=oecd_bli, right=gdp_per_capita,
                                 left_index=True, right_index=True)
    full_country_stats.sort_values(by="GDP per capita", inplace=True)
    remove_indices = [0, 1, 6, 8, 3, 34, 35]
    keep_indices = list(set(range(36)) - set(remove_indices))
    return full_country_stats[["GDP per capita",'Life satisfaction']].iloc[keep_indices]
```

그래프로 표현

```python
country_stats = prepare_country_stats(oecd_bli, gdp_per_capita)

X = np.c_[country_stats["GDP per capita"]]
y = np.c_[country_stats["Life satisfaction"]]

country_stats.plot(kind='scatter', x="GDP per capita", y='Life satisfaction')
plt.show()
```

![Example%20GDP%E1%84%8B%E1%85%AA%20Life%20Satisfaction%20a05df72d420b41918a657e4c90b68032/Untitled%201.png](Example%20GDP%E1%84%8B%E1%85%AA%20Life%20Satisfaction%20a05df72d420b41918a657e4c90b68032/Untitled%201.png)

키프로스 GDP

```python
# 키프로스 GDP
model = sklearn.linear_model.LinearRegression()
model.fit(X,y)

X_new = [[22587]]
print(model.predict(X_new))
```

![Example%20GDP%E1%84%8B%E1%85%AA%20Life%20Satisfaction%20a05df72d420b41918a657e4c90b68032/Untitled%202.png](Example%20GDP%E1%84%8B%E1%85%AA%20Life%20Satisfaction%20a05df72d420b41918a657e4c90b68032/Untitled%202.png)