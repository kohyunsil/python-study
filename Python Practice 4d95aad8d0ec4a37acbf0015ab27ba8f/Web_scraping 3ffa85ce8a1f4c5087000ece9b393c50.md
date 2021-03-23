# Web_scraping

🥑 Practice 1

```python
!pip install bs4
```

```python
from bs4 import BeautifulSoup
from urllib.request import urlopen
```

```python
url = "http://beans.itcarlow.ie/prices.html"
page = urlopen(url)
soup = BeautifulSoup(page, "html.parser")

print(soup.prettify())
```

![Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled.png](Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled.png)

```python
print(soup.find('head').prettify())

print(soup.find('body').prettify())
```

![Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%201.png](Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%201.png)

```python
print(soup.find('p').prettify())

soup.find_all('p')

soup.find_all('p')[0]

soup.find_all('p')[1]

soup.find('strong')

soup.find('strong').string

soup.find('strong').get_text()
```

![Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%202.png](Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%202.png)

🥑 Practice 2

```python
url = "https://finance.naver.com/marketindex/"
page = urlopen(url)

soup = BeautifulSoup(page, "html.parser")

print(soup.prettify())
```

```python
soup.find_all('span', 'value')
```

![Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%203.png](Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%203.png)

```python
soup.find_all('span', 'value')[0].string
```

![Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%204.png](Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%204.png)

🥑 Practice 3

```python
url = "https://book.naver.com/category/index.nhn?cate_code=280020&tab=top100&list_type=list&sort_type=publishday&page=1"
page = urlopen(url)

soup = BeautifulSoup(page, "html.parser")

print(soup.prettify())
```

```python
soup.find_all(class_='N=a:bta.title')
```

![Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%205.png](Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%205.png)

```python
soup.find_all(class_='N=a:bta.title')[0]
```

![Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%206.png](Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%206.png)

```python
# 접근주소
soup.find_all(class_='N=a:bta.title')[0]['href']

# 책 제목
soup.find_all(class_='N=a:bta.title')[0].string

# 크기
len(soup.find_all(class_='N=a:bta.title'))
```

```python
# 책 제목 저장
title = [title.string for title in soup.find_all(class_='N=a:bta.title')]
title
```

![Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%207.png](Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%207.png)

```python
# 평점 기록 (태그로 되어있지 않음)

soup.find('dd', 'txt_desc')

soup.find('dd', 'txt_desc').get_text()
```

```python
import re

tmp = soup.find('dd', 'txt_desc').get_text()

result = re.search("\d+.(\d+)?", tmp)

if result:
    print(result.group())
```

```python
score = []

for each in soup.find_all('dd', 'txt_desc'):
    result = re.search("\d+.(\d+)?", each.get_text())
    
    score.append(result.group())

score
```

![Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%208.png](Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%208.png)

```python
# 출판연도 가져오기

soup.find('dd', 'txt_block').get_text()
```

```python
tmp = soup.find('dd', 'txt_block').get_text()

result = re.search("\d+.\d+.\d+", tmp)

if result:
    print(result.group())
```

```python
date = []

for each in soup.find_all('dd', 'txt_block'):
    result = re.search("\d+.\d+.\d+", each.get_text())
    
    date.append(result.group())
    
date
```

![Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%209.png](Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%209.png)

```python
#데이터 프레임으로 나타내기
import pandas as pd

bestseller = pd.DataFrame({'책제목':title, '평점':score, '출판일':date}, index=range(1,21))
bestseller
```

![Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%2010.png](Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%2010.png)

```python
# 평점 두자리까지 나타내기
pd.options.display.float_format = '{:.2f}'.format
bestseller['평점'] = bestseller['평점'].astype(float)
bestseller
```

![Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%2011.png](Web_scraping%203ffa85ce8a1f4c5087000ece9b393c50/Untitled%2011.png)