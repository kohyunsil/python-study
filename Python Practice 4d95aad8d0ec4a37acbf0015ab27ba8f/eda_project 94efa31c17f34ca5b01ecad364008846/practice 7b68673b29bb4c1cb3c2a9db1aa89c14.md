# practice

```python
with open('data/every_weekly_webtoon.pkl', 'rb') as f:
    result = pickle.load(f)
result.tail(5)

result = result.groupby(['rank', 'star', 'img','person']).size().reset_index()[['rank','star','img','person']]
result
```

```python
a = result[result['rank']==56]
b = result[result['rank']==57]
c = result[result['rank']==58]
d = result[result['rank']==59]
e = result[result['rank']==60]
f = result[result['rank']==61]

dfdf = pd.concat([a,b,c,d,e,f],axis=0)

df = dfdf.corr().round(4)
star_img = df.iloc[1, 2]
star_person = df.iloc[1, 3]
img_person = df.iloc[2, 3]
df
```

```python
### rank1 = rank 1 ~ 5
star_img, star_person, img_person
```