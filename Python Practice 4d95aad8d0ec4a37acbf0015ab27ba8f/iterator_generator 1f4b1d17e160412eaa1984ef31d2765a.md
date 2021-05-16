# iterator_generator

- iterable : 순서가 있는 데이터 집합
- iterator : 순서가 있는 데이터를 만드는 값 생성기
- generator : iterator를 간단하게 구현할 수 있도록 하는 문법

```markdown
# 1. iterator 만들기 1
# iter() 함수 사용
```

```python
ls = [1, 2, 3]

iterator = iter(ls)

type(iterator)

next(iterator)
```

```python
# 2. iterator 만들기 2
# class 사용

class Fib:
    def __init__(self):
        self.prev = 0
        self.curr = 1
    
    def __next__(self):
        value = self.curr
        self.curr += self.prev
        self.prev = value
        return value

fib = Fib()

next(fib)
```

```python
# 3. generator

def fib():
    prev, curr = 0, 1
    while True:
        yield curr
        prev, curr = curr, prev + curr

f = fib()

type(f)

next(f)

def test():
    yield 1
    yield 2    
    yield 3

t = test()

next(t)
```

comprehension : list, set, dict

```python
# set comprehesion
ls = [1, 2, 3, 4, 5]
data = {num % 2 for num in ls}
type(data), data
```

![iterator_generator%201f4b1d17e160412eaa1984ef31d2765a/Untitled.png](iterator_generator%201f4b1d17e160412eaa1984ef31d2765a/Untitled.png)

```python
# dict comprehesion
ls = [1, 2, 3, 4, 5]
data = {num: num % 2 for num in ls}
type(data), data
```

```python
# 제너레이터 표현식

ls = [1, 2, 3, 4, 5]
data = (num % 2 for num in ls)
type(data), data
```

```python
# set comprehesion
ls = [1, 2, 3, 4, 5]
data = {num % 2 for num in ls}
type(data), data
```

![iterator_generator%201f4b1d17e160412eaa1984ef31d2765a/Untitled%201.png](iterator_generator%201f4b1d17e160412eaa1984ef31d2765a/Untitled%201.png)