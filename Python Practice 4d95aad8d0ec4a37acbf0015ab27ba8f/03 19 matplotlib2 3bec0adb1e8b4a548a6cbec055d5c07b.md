# 03/19 matplotlib2

bar

```python
# bar
datas = np.random.randint(10, size=5)
plt.bar(range(len(datas)), datas, width=0.3)
plt.show()
```

ragb(FFF) : 16진수 0 ~ F ( a=alpha, 투명도설정가능)

```python
datas = np.random.randint(5, 10, size=5)
error = np.random.randint(1, 5, size=5)
plt.bar(range(len(datas)), datas, width=0.4, alpha=0.4, yerr=error)
plt.show()
```

![03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled.png](03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled.png)

```python
datas = np.random.randint(5, 10, size=5)
error = np.random.randint(1, 5, size=5)
plt.barh(range(len(datas)), datas, alpha=0.4, xerr=error)
plt.show()
```

![03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%201.png](03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%201.png)

```python
datas_1 = np.random.randint(5, 10, size=5)
datas_2 = np.random.randint(1, 5, size=5)
plt.bar(range(len(datas_2)), datas_2)
plt.bar(range(len(datas_1)), datas_1)
plt.show()
```

![03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%202.png](03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%202.png)

```python
datas_1 = np.random.randint(5, 10, size=5)
datas_2 = np.random.randint(1, 5, size=5)
plt.bar(range(len(datas_2)), datas_1 + datas_2)
plt.bar(range(len(datas_1)), datas_1)
plt.show()
```

![03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%203.png](03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%203.png)

pie 차트

```python
plt.pie(datas, labels=list("ABCDE"), autopct="%1.2f%%")
plt.show()
```

![03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%204.png](03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%204.png)

scatter plot

- numpy : random : rand(균등), randn(정규), randint(균등분포)

```python
x = np.random.rand(500)
y = np.random.rand(500)
plt.scatter(x, y)
plt.show()
```

![03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%205.png](03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%205.png)

```python
x = np.random.randn(500)
y = np.random.randn(500)
plt.scatter(x, y)
plt.show()
```

![03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%206.png](03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%206.png)

```python
x = np.random.randint(100, size=500)
y = np.random.randint(100, size=500)
plt.scatter(x, y)
plt.show()
```

![03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%207.png](03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%207.png)

```python
x = np.random.randint(10, size=20)
y = np.random.randint(10, size=20)
colors = np.random.rand(20)
area = np.pi * np.arange(10, 50, 2) ** 2
plt.scatter(x, y, c=colors, s=area, alpha=0.5)
plt.show()
```

![03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%208.png](03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%208.png)

histogram

```python
y = np.random.rand(100)
plt.hist(y)
plt.show()
```

![03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%209.png](03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%209.png)

Seaborn

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

![03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%2010.png](03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%2010.png)

```python
sns.set_style("whitegrid") # default가 darkgrid
plt.plot(datas)
plt.show()
```

![03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%2011.png](03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%2011.png)

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

![03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%2012.png](03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%2012.png)

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

![03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%2013.png](03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%2013.png)

```python
sns.pairplot(iris, hue="species")
plt.show().  # 각 컬럼별로 산점도를 그려준다.
```

![03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%2014.png](03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%2014.png)

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

![03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%2015.png](03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%2015.png)

```python
sns.heatmap(df, cmap="YlGnBu", annot=True, fmt="d")
plt.show()
```

![03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%2016.png](03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%2016.png)

```python
sns.heatmap(iris.corr(), cmap="YlGnBu", annot=True)
plt.show()
```

![03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%2017.png](03%2019%20matplotlib2%203bec0adb1e8b4a548a6cbec055d5c07b/Untitled%2017.png)