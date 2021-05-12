# Selenium

Selenium¶

```markdown
Selenium¶

	웹개발 테스트용으로 만들어짐
	브라우져를 자동화하는 라이브러리
	다양한 언어와 다양한 브러우져를 지원

환경설정
	크롬브라우져 설치
	크롬드라이버 다운로드 및 설정
		크롬브라우져 버젼 확인 : ver.90
		크롬드라이버 다운로드
			chrome driver download 검색
			버젼, OS에 맞는 드라이버 다운로드
			환경변수설정 : 모든 디렉토리에서 사용가능
				mac
					$ mv ~/Download/chromedriver /usr/local/bin
selenium 패키지 설치
	$ pip install selenium
	$ conda install selenium (anaconda use)
		$ conda instaㅣl -c conda-forge selenium
```

```python
from selenium import webdriver

# 브라우져 띄우기
driver = webdriver.Chrome()

# 1. 페이지 이동
driver.get("https://daum.net")

# 2. 브라우져 크기조절
driver.set_window_size(800,600)

# 3. 브라우져 스크롤 조절
driver.execute_script("window.scrollTo(200,300);")
driver.execute_script("alert('dss!!!');")

# 4. alert 다루기
# alert 확인 버튼 클릭
alert = driver.switch_to.alert
alert

alert.accept()

# 5. input에 문자열 입력
# find_element_by_css_selector : select_one()
# find_elements_by_css_selector : select()
driver.find_element_by_css_selector("#q").clear()
driver.find_element_by_css_selector("#q").send_keys("파이썬")

# 6. 버튼 클릭
selector = "#daumSearch > fieldset > div > div > button.ico_pctop.btn_search"
driver.find_element_by_css_selector(selector).click()

# 7. 브라우져 종료
driver.quit()
```

예제 : ted 사이트 크롤링

```python
# select box event, text데이터 가져오기 등등

# 브라우져 열기 및 페이지 이동
# https://www.ted.com/talks
driver = webdriver.Chrome()
driver.get("https://www.ted.com/talks")

# text 데이터 가져오기
sub_title = driver.find_element_by_css_selector("#banner-secondary").text
sub_title

# 목록에서 영상의 제목 데이터 수집
elements = driver.find_elements_by_css_selector("#browse-results > .row > .col")
len(elements)
element = elements[0]
# title = element.find_element_by_css_selector("h4.h12").text
titles = [element.find_element_by_css_selector("h4.h12").text for element in elements]
titles[:5]

# 목록에서 영상의 링크 데이터 수집
element = elements[0]
links = [element.find_element_by_css_selector("h4 > a.ga-link").get_attribute("href") for element in elements]

pd.DataFrame({"title": titles,"link" : links}).tail(2)
```

![Selenium%20fd9b6728b41a4410bfcb000a04355cbf/Untitled.png](Selenium%20fd9b6728b41a4410bfcb000a04355cbf/Untitled.png)