# 03/06 numpy - Correlation Coefficient

상관계수 ( Correlation Coefficient )

- numpy를 이용하여 데이터의 상관계수를 구합니다.
- python 코드와 numpy의 함수의 속도차이를 비교합니다.

분산(variance)

- 1개의 이산정도를 나타냄
- 편차제곱의 평균

```python
# variance code
def variance(datas):
    var = 0
    x_ = np.mean(datas)
    
    for data in datas:
        var += (data - x_) ** 2
    
    
    return var / len(datas)

# 함수 호출
variance(data1), variance(data2),variance(data1) ** 0.5, variance(data2)** 0.5
np.var(data1), np.var(data2), np.std(data1), np.std(data2)
```

일반 함수와 numpy 함수의 퍼포먼스 비교

```python
p_data1 = np.random.randint(60, 100, int(1E5))
p_data2 = np.random.randint(60, 100, int(1E5))

%%time
variance(p_data1), variance(p_data2)

### out
# CPU times: user 650 ms, sys: 13.6 ms, total: 663 ms
#Wall time: 699 ms
# (133.64774725759224, 133.18345171039562)

%%time
np.var(p_data1), np.var(p_data2)

### out
# CPU times: user 1.95 ms, sys: 1.02 ms, total: 2.97 ms
# Wall time: 2.37 ms
# (133.6477472576, 133.1834517104)
```

 공분산(covariance)

- 2개의 확률변수의 상관정도를 나타냄
- 평균 편차곱
- 방향성은 보여줄수 있으나 강도를 나타내는데 한계가 있다
    - 표본데이터의 크기에 따라서 값의 차이가 큰 단점이 있다

```python
data1 = np.array([80, 85, 100, 90, 95])
data2 = np.array([70, 80, 100, 95, 95])
data3 = np.array([100, 90, 70, 90, 80])

#호출
np.cov(data1, data2)[0, 1], np.cov(data1, data3)[0, 1]

data4 = data1 * 10
data5 = data3 * 10
data4, data5

#호출
np.cov(data4, data5)[0, 1]
```

 상관계수

- 공분산의 한계를 극복하기 위해서 만들어짐
- 1 ~ 1까지의 수를 가지며 0과 가까울수록 상관도가 적음을 의미
- x의 분산과 y의 분산을 곱한 결과의 제곱근을 나눠주면 x나 y의 변화량이 클수록 0에 가까워짐
- [https://docs.scipy.org/doc/numpy-1.13.0/reference/generated/numpy.corrcoef.html](https://docs.scipy.org/doc/numpy-1.13.0/reference/generated/numpy.corrcoef.html)

```python
np.corrcoef(data1, data3)[0, 1], np.corrcoef(data4, data5)[0, 1]
```

 결정계수(cofficient of determination: R-squared)

- x로부터 y를 예측할수 있는 정도
- 상관계수의 제곱 (상관계수를 양수화)
- 수치가 클수록 회기분석을 통해 예측할수 있는 수치의 정도가 더 정확

```python
np.corrcoef(data1, data3)[0, 1] ** 2, np.corrcoef(data1, data2)[0, 1] ** 2
```