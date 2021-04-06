# def & class practice

christmas present search -  ver.def

```python
# 크리스마스 선물 찾기

import pandas as pd
import numpy as np

import platform
import matplotlib.pyplot as plt

%matplotlib inline

from matplotlib import rc
rc('font', family='AppleGothic')
plt.rcParams['axes.unicode_minus']=False

from bs4 import BeautifulSoup
from urllib.request import urlopen, Request
import requests
import urllib
import time

def get_url(insert):
    html = 'http://kin.naver.com/search/list.nhn?query={key_word}&page={num}'
    req = Request(html.format(num=1, key_word=urllib.parse.quote(insert)));
    req.add_header('Referer', 'http://www.naver.com/')
    response = urlopen(req)
    soup = BeautifulSoup(response, "html.parser")
    return soup

def get_links(soup):
    links = soup.find_all('dt')
    return links

def get_soup_tmp(links):
    links[0]
    links[0].a['href']
    html = links[0].a['href']
    r = requests.get(links[0].a['href'])
    soup_tmp = BeautifulSoup(r.text, 'html.parser')
    soup_tmp = soup_tmp.find_all('div', '_endContentsText')
    return soup_tmp

def get_contents(soup_tmp):
    contents = []
    search_result = soup_tmp
    for each in search_result:
        contents.append(each.get_text())

    return contents
```

```python
def get_url_link(insert):
    soup = get_url(insert)
    links = get_links(soup)
    soup_tmp = get_soup_tmp(links)
    contents = get_contents(soup_tmp)
    
    return contents
```

```python
result = get_url_link('크리스마스선물')
result
```

```python
def get_present_candi_text(links):
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
    return present_candi_text
```

```python
def get_url_link(insert):
    soup = get_url(insert)
    links = get_links(soup)
    result = get_present_candi_text(links)
    return result
```

```python
result = get_url_link('크리스마스선물')
result
```