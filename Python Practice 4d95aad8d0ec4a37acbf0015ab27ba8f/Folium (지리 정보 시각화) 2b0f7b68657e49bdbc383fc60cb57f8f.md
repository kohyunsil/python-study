# Folium (지리 정보 시각화)

- folium : leaflet.js javascript에서 사용하는 지리정보 라이브러리를 python 버전으로 변환
    - !pip install folium==0.11.0 (설치)
- geo pandas

지도 그리기

```python
import folium

location = np.array([37.5665, 126.9780])
fmap = folium.Map(location, zoom_start=16) # zoom : 0 ~ 18
fmap
```

마커추가

```python
html = "<a href='https://google.com' target='_blink'>GO!<br>GOOLGE<\a>"
folium.Marker(location, popup=html)
fmap
```

마커 icon 설정

```python
# 마커 Icon 설정 
folium.Marker(location - 0.001, icon=folium.Icon(color="red", icon="cloud"))
fmap
```

circle 추가

```python
# circle 추가
fmap = folium.Map(location, zoom_start=16)
folium.CircleMarker(location, radius=30, color="blue", fill=True, fill_color="red", toltip="center").add_to(fmap)
fmap
```

스타일 설정

```python

```

주별 미국 실업률 데이터

- matplotlib 기반의 python 시각화 라이브러리
- 고급 인터페이스를 제공

```python
import seaborn as sns

#seaborn 스타일 적용
sns.set()
```

```python
plt.plot(datas)
plt.show()
```

```python
sns.set_style("whitegrid") # default가 darkgrid
plt.plot(datas)
plt.show()
```

컬러 테마 변경

```python
# 기본
sns.palplot(sns.color_palette())

# theme
theme = sns.color_palette("hls", 16)
theme

# 적용
sns.set_palette("hls",10)
sns.set_style("whitegrid")
for _ in range(5):
    plt.plot(np.random.randint(5, size=5))
plt.show()
```

Examples

```python
# iris
iris = sns.load_dataset("iris")
iris.tail(2)
```

lmplot

```python
sns.lmplot("sepal_length", "sepal_width", data=iris, hue="species")
plt.show()
```

```python
sns.pairplot(iris, hue="species")
plt.show().  # 각 컬럼별로 산점도를 그려준다.
```

heat map : 컬럼별 상관계수 출력시 자주 사용

```python
flights = sns.load_dataset("flights")
df = flights.pivot("month", "year", "passengers")
df
```

```python
# heatmap
sns.heatmap(df)
plt.show()
```

```python
sns.heatmap(df, cmap="YlGnBu", annot=True, fmt="d")
plt.show()
```

```python
sns.heatmap(iris.corr(), cmap="YlGnBu", annot=True)
plt.show()
```