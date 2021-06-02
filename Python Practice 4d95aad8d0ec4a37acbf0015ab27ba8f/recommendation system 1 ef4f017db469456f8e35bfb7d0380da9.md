# recommendation system 1

데이터 읽기

- kaggle 👉🏻 tmdb_5000_movies dataset

![recommendation%20system%201%20ef4f017db469456f8e35bfb7d0380da9/_2021-06-02__11.36.51.png](recommendation%20system%201%20ef4f017db469456f8e35bfb7d0380da9/_2021-06-02__11.36.51.png)

문자열 데이터 변환

```python
# practice

from ast import literal_eval

code = """(1, 2, {'foo': 'bar'})"""
code

##### type(code) 👉🏻 str

literal_eval(code)

##### type(code) 👉🏻 tuple
```

- genres와 keywords의 내용을 list와 dict으로 복구

```python
from ast import literal_eval

movies_df['genres'] = movies_df['genres'].apply(literal_eval)
movies_df['keywords'] = movies_df['keywords'].apply(literal_eval)
movies_df['genres'] = movies_df['genres'].apply(lambda x : [y['name'] for y in x])
movies_df['keywords'] = movies_df['keywords'].apply(lambda x : [y['name'] for y in x])
movies_df['genres_literal'] = movies_df['genres'].apply(lambda x : (' ').join(x))
movies_df.head()
```

![recommendation%20system%201%20ef4f017db469456f8e35bfb7d0380da9/Untitled.png](recommendation%20system%201%20ef4f017db469456f8e35bfb7d0380da9/Untitled.png)

문자열로 변환된 genres_literal를 CountVectorize 수행

```python
from sklearn.feature_extraction.text import CountVectorizer

count_vect = CountVectorizer(min_df=0, ngram_range=(1,2))
genre_mat = count_vect.fit_transform(movies_df['genres_literal'])
print(genre_mat.shape)
```

문장의 유사도 측정을 하는 방법 중 하나인 코사인 유사도 측정

![recommendation%20system%201%20ef4f017db469456f8e35bfb7d0380da9/Untitled%201.png](recommendation%20system%201%20ef4f017db469456f8e35bfb7d0380da9/Untitled%201.png)

```python
from sklearn.metrics.pairwise import cosine_similarity

genre_sim = cosine_similarity(genre_mat, genre_mat)
print(genre_sim.shape)
print(genre_sim[:2])
```

![recommendation%20system%201%20ef4f017db469456f8e35bfb7d0380da9/_2021-06-02__11.49.44.png](recommendation%20system%201%20ef4f017db469456f8e35bfb7d0380da9/_2021-06-02__11.49.44.png)

- genre_sim 객체에서 높은 값 순으로 정렬

```python
genre_sim_sorted_ind = genre_sim.argsort()[:, ::-1]
print(genre_sim_sorted_ind[:1])
```

- 추천 영화를 DataFrame으로 반환하는 함수

```python
def find_sim_movie(df, sorted_ind, title_name, top_n=10):
    title_movie = df[df['title'] == title_name]
    
    title_index = title_movie.index.values
    similar_indexes = sorted_ind[title_index, :(top_n)]
    
    print(similar_indexes)
    similar_indexes = similar_indexes.reshape(-1)
    
    return df.iloc[similar_indexes]
```

대부와 비슷한 영화

![recommendation%20system%201%20ef4f017db469456f8e35bfb7d0380da9/Untitled%202.png](recommendation%20system%201%20ef4f017db469456f8e35bfb7d0380da9/Untitled%202.png)

- 평점과 평점을 매긴 횟수를 보면 문제 데이터를 볼 수 있다.

가중치 부여

- 영화 전체 평균평점과 최소 투표 횟수를 60%지점으로 지정

```python
C = movies_df['vote_average'].mean()
m = movies_df['vote_count'].quantile(0.6)
print('C:',round(C,3), 'm:',round(m,3))
```

- 가중치가 부여된 평점을 계산하기 위한 함수

```
def weighted_vote_average(record):
    v = record['vote_count']
    R = record['vote_average']    
    
    return ( (v/(v+m)) * R ) + ( (m/(m+v)) * C )

movies_df['weighted_vote'] = movies_df.apply(weighted_vote_average, axis=1)
```

- 추천 영화를 DataFrame으로 반환하는 함수 변경 ( 'weighted_vote')

```python
def find_sim_movie(df, sorted_ind, title_name, top_n=10):
    title_movie = df[df['title'] == title_name]
    title_index = title_movie.index.values
    
    similar_indexes = sorted_ind[title_index, :(top_n*2)]
    similar_indexes = similar_indexes.reshape(-1)

    similar_indexes = similar_indexes[similar_indexes != title_index]
    
    return df.iloc[similar_indexes].sort_values('weighted_vote',
                                               ascending=False)[:top_n]
```

![recommendation%20system%201%20ef4f017db469456f8e35bfb7d0380da9/_2021-06-02__11.58.11.png](recommendation%20system%201%20ef4f017db469456f8e35bfb7d0380da9/_2021-06-02__11.58.11.png)