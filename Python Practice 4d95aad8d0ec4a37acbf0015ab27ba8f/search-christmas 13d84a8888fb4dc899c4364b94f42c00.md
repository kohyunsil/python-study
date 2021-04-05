# search-christmas

christmas present search

```python
import pandas as pd
import numpy as np

import platform
import matplotlib.pyplot as plt

%matplotlib inline

from matplotlib import rc
rc('font', family='AppleGothic')
plt.rcParams['axes.unicode_minus']=False
```

```python
from bs4 import BeautifulSoup
from urllib.request import urlopen, Request
import requests
import urllib
import time
```

```python
links = soup.find_all('dt')
links[0]
links[0].a['href']
```

```python
html = links[0].a['href']

r = requests.get(links[0].a['href'])
soup_tmp = BeautifulSoup(r.text, 'html.parser')

soup_tmp.find_all('div', '_endContentsText')
```

```python
contents = []

search_result = soup_tmp.find_all('div', '_endContentsText')

for each in search_result:
    contents.append(each.get_text())
    
contents
```

```python
from tqdm.notebook import tqdm
import time

present_candi_text = []

for each_link in tqdm(links):
    r = requests.get(each_link.a['href'])
    soup_tmp = BeautifulSoup(r.text, 'html.parser')
    
    search_result = soup_tmp.find_all('div', '_endContentsText')
    
    time.sleep(0.1)
    
    for each in search_result:
        present_candi_text.append(each.get_text())
```

```python
present_candi_text
```

```python
import nltk
from konlpy.tag import Okt; t = Okt()

present_text = ''

for each_line in present_candi_text:
    present_text = present_text + each_line + '\n'

tokens_ko = t.nouns(present_text)
tokens_ko
```

```python
ko = nltk.Text(tokens_ko, name='크리스마스 선물')
print(len(ko.tokens))
print(len(set(ko.tokens)))
```

```python
ko = nltk.Text(tokens_ko, name='크리스마스 선물')
ko.vocab().most_common(100)
```

```python
stop_words = ['한', '수', '은', '들', '!', '스', '용', '이', '빔', ',', '.','제',
             '것', '저', '거', '뼘', '린', '중', '분', '요', '해', '때문', '줄', '는']

tokens_ko = [each_word for each_word in tokens_ko if each_word not in stop_words]

ko = nltk.Text(tokens_ko, name = '여자 친구 선물')
ko.vocab().most_common(50)
```

```python
plt.figure(figsize=(15,6))
ko.plot(50)
plt.show()
```

![search-christmas%2013d84a8888fb4dc899c4364b94f42c00/Untitled.png](search-christmas%2013d84a8888fb4dc899c4364b94f42c00/Untitled.png)

```python
from wordcloud import WordCloud, STOPWORDS
from PIL import Image
```

```python
data = ko.vocab().most_common(300)

wordcloud = WordCloud(font_path='/Library/Fonts/Arial Unicode.ttf',
                     relative_scaling=0.5,
                     background_color='white',
                     ).generate_from_frequencies(dict(data))
plt.figure(figsize=(16,8))
plt.imshow(wordcloud)
plt.axis("off")
plt.show()
```

![search-christmas%2013d84a8888fb4dc899c4364b94f42c00/Untitled%201.png](search-christmas%2013d84a8888fb4dc899c4364b94f42c00/Untitled%201.png)

```python
from wordcloud import ImageColorGenerator

mask = np.array(Image.open('datas/heart.jpg'))
image_colors = ImageColorGenerator(mask)
```

```python
data = ko.vocab().most_common(200)

wordcloud = WordCloud(font_path='/Library/Fonts/Arial Unicode.ttf',
                     relative_scaling=0.1, mask=mask,
                     background_color='white',
                      min_font_size=1, max_font_size=100
                     ).generate_from_frequencies(dict(data))

default_colors = wordcloud.to_array()
```

```python
plt.figure(figsize=(12,12))
plt.imshow(wordcloud.recolor(color_func=image_colors),
          interpolation='bilinear')
plt.axis("off")
plt.show()
```

![search-christmas%2013d84a8888fb4dc899c4364b94f42c00/Untitled%202.png](search-christmas%2013d84a8888fb4dc899c4364b94f42c00/Untitled%202.png)