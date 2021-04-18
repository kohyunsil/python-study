# practice Eda

month_genre : 월별 인기 장르

```python
import pickle
import pandas as pd
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt
import seaborn as sns
```

```python
from matplotlib import font_manager, rc

import matplotlib.pyplot as plt
%matplotlib inline

from matplotlib import font_manager
font_dirs = ['/Library/Fonts']
font_files = font_manager.findSystemFonts(fontpaths=font_dirs)
font_list = font_manager.createFontList(font_files)
font_manager.fontManager.ttflist.extend(font_list)

import seaborn as sns
sns.set(font='AppleGothic')
```

```python
# title 공백 없애주는 함수
def title_strip(title):
    return title.strip()
```

```python
yearly_df.drop(columns='link', axis=1, inplace=True)

yearly_df['title'] = yearly_df['title'].apply(title_strip)
yearly_df['img'] = yearly_df['img'].astype('int')
yearly_df['person'] = yearly_df['person'].astype('int')
yearly_df['date'] = pd.to_datetime(yearly_df['date'], format='%Y-%m-%d', errors='raise')

yearly_df.rename(columns={'date':'datetime'}, inplace=True)

yearly_df.head()
```

```python
yearly_df['genre1'] = yearly_df['genre'].apply(split_genre1)
yearly_df['genre2'] =yearly_df['genre'].apply(split_genre2)
```

```python
yearly_df = yearly_df[[
    'year', 'title', 'genre', 'genre1', 'genre2', 'star', 'img', 'person', 'datetime', 'date_year', 'quarter', 'month', 'day']]
yearly_df.tail(2)
```

```python
def yearly_month_genre(months):
    result_df = yearly_df[yearly_df['month'] == months]
    person_mean = result_df.groupby('title').agg('mean').round(2)[['person']].reset_index()
    result_s = result_df.groupby(['title','genre2']).size().reset_index()
    result_s.drop(columns = 0, axis=1, inplace=True)
    month_genre = pd.concat([result_s, person_mean['person']], axis = 1, join="inner")
    month_genre = month_genre.groupby('genre2').agg('mean').round(2)[['person']].reset_index()
    month_genre.sort_values('person', ascending=False, inplace=True)
    return month_genre
```

```python
month_genres = []
for a in range(1, 13):
    r = yearly_month_genre(a)
    month_genres.append(r)
    
df = pd.DataFrame()
for i in range(len(month_genres)):
    df = pd.concat([df, month_genres[i]['person']],1)
```

```python
df.columns = ['1월', '2월', '3월', '4월', '5월', '6월', '7월', '8월', '9월', '10월', '11월', '12월']
df
```

```python
month_df = pd.concat([month_genres[0]['genre2'], df], 1)
month_df = month_df.set_index("genre2")
# month_df.fillna("X")
# month_df = month_df.T
month_df
```

```python
ax = month_df.plot(kind="bar", width=0.6)
fig = ax.get_figure()
fig.set_size_inches(20, 10)
ax.set_xlabel("month")
ax.set_ylabel("월별 인기 장르")
plt.xticks(rotation=1)
plt.legend(bbox_to_anchor=(1.1, 1))
plt.show()
```

![practice%20Eda%20ae6f489817944236827ceb80bf3e8af9/Untitled.png](practice%20Eda%20ae6f489817944236827ceb80bf3e8af9/Untitled.png)

```python
maxs = month_df.max(axis=0)
maxs = pd.DataFrame(maxs)
maxs.columns=['max']
maxs
```

```python
month_df = month_df.T
month_df.index = month_df.index.str.replace("월", "")

value = []
genre = []
for i in range(len(month_df)):
    value.append(month_df.iloc[i].max())
    genre.append(month_df.iloc[i].idxmax())
    
df = pd.DataFrame(
                {'value' : value,
                 'genre' : genre},
                 index = range(len(value)))
df["월"] = range(1,13)
df
```

```python
plt.figure(figsize=(12,6))
g = sns.barplot(data=df, x='월', y ='value', hue='genre', dodge=False)
g.text
plt.title('월별 인기 웹툰 장르', fontsize=20)
plt.legend(bbox_to_anchor=(1.05, 1))
plt.show()
```

![practice%20Eda%20ae6f489817944236827ceb80bf3e8af9/Untitled%201.png](practice%20Eda%20ae6f489817944236827ceb80bf3e8af9/Untitled%201.png)