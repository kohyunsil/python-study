# 01/27 python new quiz

- 영화 좌석 퀴즈

```python
for i in range(1,21):
  if i % 2 == 1:
    print("A"+str(i), end=" ")
print()

for i in range(1,21)[::3]:
  print("A"+str(i),end=" ")
```