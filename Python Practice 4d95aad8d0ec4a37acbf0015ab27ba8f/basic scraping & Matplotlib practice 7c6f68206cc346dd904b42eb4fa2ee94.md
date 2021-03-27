# basic scraping & Matplotlib practice

import

```python
import pandas as pd
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt
```

BeautifulSoup

```python
from bs4 import BeautifulSoup
from urllib.request import urlopen

url = "https://comic.naver.com/webtoon/weekdayList.nhn?week=mon"
page = urlopen(url)
soup = BeautifulSoup(page, "html.parser")
```

```python
# 요일별 평점 가져오기

monday_score = []
a = soup.select('div.rating_type > strong')

for i in a:
    b = i.text
    monday_score.append(b)
```

```python
# mon ~ sun 
mon = np.array(monday_score).astype('float32')
mon = np.mean(mon)
mon
```

matplotlib을 위한 데이터 정리

```python
datas = {"monday":mon, "tuesday":tue, "wednesday":wed, "thursday":thu, "friday":fri, "saturday":sat, "sunday":sun}
keys = datas.keys()
values = datas.values()
```

데이터 프레임 만들기

```python
df = pd.DataFrame(datas, index=[0])
df
```

![basic%20scraping%20&%20Matplotlib%20practice%207c6f68206cc346dd904b42eb4fa2ee94/Untitled.png](basic%20scraping%20&%20Matplotlib%20practice%207c6f68206cc346dd904b42eb4fa2ee94/Untitled.png)

bar 그리기

```python
plt.figure(figsize=(10, 5))
plt.bar(keys, values)
plt.ylim(9.5, 10)
plt.show()
```

![basic%20scraping%20&%20Matplotlib%20practice%207c6f68206cc346dd904b42eb4fa2ee94/Untitled%201.png](basic%20scraping%20&%20Matplotlib%20practice%207c6f68206cc346dd904b42eb4fa2ee94/Untitled%201.png)