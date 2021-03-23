# Box Plot, Scalers

데이터 로드

```python
import matplotlib.pyplot as plt

samples = [1, 7, 9, 16, 36, 39, 45, 45, 46, 48, 51, 100, 101]
tmp_y = [1]*len(samples)
tmp_y
```

```python
plt.figure(figsize=(12,4))
plt.scatter(samples, tmp_y)
plt.grid()
plt.show()
```

![Box%20Plot,%20Scalers%20973e02ea6f8346739fe0add4c1751400/Untitled.png](Box%20Plot,%20Scalers%20973e02ea6f8346739fe0add4c1751400/Untitled.png)

지표 찾기, 그리기(Box Plot)

```python
np.median(samples)
#45.0

np.percentile(samples, 25)
#16.0

np.percentile(samples, 75)
#48.0

np.percentile(samples, 75) - np.percentile(samples, 25)
#32.0

iqr = np.percentile(samples, 75) - np.percentile(samples, 25)
iqr*1.5
#48.0
```

```python
q1 = np.percentile(samples, 25)
q2 = np.median(samples)
q3 = np.percentile(samples, 75)

upper_fence = q3 + iqr*1.5
lower_fence = q1 - iqr*1.5
```

```python
plt.figure(figsize=(12,4))
plt.scatter(samples, tmp_y)
plt.axvline(x=q1, color='black')
plt.axvline(x=q2, color='red')
plt.axvline(x=q3, color='black')
plt.axvline(x=upper_fence, color='black', ls='dashed')
plt.axvline(x=lower_fence, color='black', ls='dashed')
plt.grid()
plt.show()
```

![Box%20Plot,%20Scalers%20973e02ea6f8346739fe0add4c1751400/Untitled%201.png](Box%20Plot,%20Scalers%20973e02ea6f8346739fe0add4c1751400/Untitled%201.png)

```python
import seaborn as sns
plt.figure(figsize=(12,4))
#sns.boxplot(samples)
sns.boxplot(data=samples, orient='h')
plt.grid()
plt.show()
```

![Box%20Plot,%20Scalers%20973e02ea6f8346739fe0add4c1751400/Untitled%202.png](Box%20Plot,%20Scalers%20973e02ea6f8346739fe0add4c1751400/Untitled%202.png)

# Scalers

dataset

```python
import pandas as pd
df = pd.DataFrame({
    'A' : ['a', 'b', 'c', 'a', 'b'],
    'B' : [1, 2, 3, 1, 0]
})
df
```

```python
#label encoder란?

from sklearn.preprocessing import LabelEncoder

le = LabelEncoder()
le.fit(df['A'])
```

```python
le.classes_
```

```python
# column A의 문자를 숫자- 카테고리컬한 데이터로 변환

df['le_A'] = le.transform(df['A'])
df
```

fit & transform, inverse_transform

```python
le.transform(['a', 'b'])

le.fit_transform(df['A'])

#역변환
le.inverse_transform([1,2,2,2])
```

Min-Max scaling

```python
df = pd.DataFrame({
    'A' : [10, 20, -10, 0, 25],
    'B' : [1, 2, 3, 1, 0]
})

df
```

```python
from sklearn.preprocessing import MinMaxScaler

mms = MinMaxScaler()
mms.fit(df)
```

```python
mms.data_max_, mms.data_min_
```

```python
df_mms = mms.transform(df)
df_mms
```

```python
mms.inverse_transform(df_mms)
```

```python
mms.fit_transform(df)
```

![Box%20Plot,%20Scalers%20973e02ea6f8346739fe0add4c1751400/Untitled%203.png](Box%20Plot,%20Scalers%20973e02ea6f8346739fe0add4c1751400/Untitled%203.png)

```python
from sklearn.preprocessing import StandardScaler

ss = StandardScaler()
ss.fit(df)
```

```python
ss.mean_, ss.scale_
```

```python
df_ss = ss.transform(df)
df_ss

ss.fit_transform(df)

ss.inverse_transform(df_ss)
```

```python
df = pd.DataFrame({
    'A':[-0.1, 0., 0.1, 0.2, 0.3, 0.4, 1.0, 1.1, 5]
})

from sklearn.preprocessing import MinMaxScaler, StandardScaler, RobustScaler

mm = MinMaxScaler()
ss = StandardScaler()
rs = RobustScaler()

df_scaler = df.copy()

df_scaler['MinMax'] = mm.fit_transform(df)
df_scaler['Standard'] = ss.fit_transform(df)
df_scaler['Robust'] = rs.fit_transform(df)

df_scaler
```

![Box%20Plot,%20Scalers%20973e02ea6f8346739fe0add4c1751400/Untitled%204.png](Box%20Plot,%20Scalers%20973e02ea6f8346739fe0add4c1751400/Untitled%204.png)

```python
import seaborn as sns
import matplotlib.pyplot as plt
sns.set_theme(style="whitegrid")
plt.figure(figsize=(16, 6))
sns.boxplot(data=df_scaler, orient='h');
```

![Box%20Plot,%20Scalers%20973e02ea6f8346739fe0add4c1751400/Untitled%205.png](Box%20Plot,%20Scalers%20973e02ea6f8346739fe0add4c1751400/Untitled%205.png)