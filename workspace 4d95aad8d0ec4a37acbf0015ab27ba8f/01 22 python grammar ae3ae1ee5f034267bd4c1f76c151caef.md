# 01/22 python grammar

```python
scores=[[90,80,90],[85,85,95],[95,70,75],[80,80,90],[90,75,80]]

print("구현방법(A)")
for row in range(len(scores)):
    sum = 0
    for col in range(len(scores[row])):
        sum +=scores[row][col]; 
    avg=sum/len(scores[row])
    print(sum,round(avg,2))

print("구현방법(b)")
for row in scores:
    sum = 0
    for element in row:
        sum +=element 
    avg=sum/len(row)
    print(sum,round(avg,2))
```

![01%2022%20python%20grammar%20ae3ae1ee5f034267bd4c1f76c151caef/Untitled.png](01%2022%20python%20grammar%20ae3ae1ee5f034267bd4c1f76c151caef/Untitled.png)