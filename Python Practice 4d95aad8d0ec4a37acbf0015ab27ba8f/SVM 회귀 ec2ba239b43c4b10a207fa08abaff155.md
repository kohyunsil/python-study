# SVM 회귀

Margin

![SVM%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%20ec2ba239b43c4b10a207fa08abaff155/Untitled.png](SVM%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%20ec2ba239b43c4b10a207fa08abaff155/Untitled.png)

서포트 벡터 머신

- 경계의 양측 거리 즉, margin이 최대가 되도록 경계면을 찾는 알고리즘 → 서포트벡터머신
- 선형, 비선형 분류, 회귀, 이상치 탐색에도 괜찮은 다목적 머신러닝 모델
- 복잡한 분류 문제에 잘 맞으며 데이터량이 적어도 성능이 괜찮은 것으로 알려짐

![SVM%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%20ec2ba239b43c4b10a207fa08abaff155/Untitled%201.png](SVM%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%20ec2ba239b43c4b10a207fa08abaff155/Untitled%201.png)

소프트 마진 분류

![SVM%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%20ec2ba239b43c4b10a207fa08abaff155/Untitled%202.png](SVM%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%20ec2ba239b43c4b10a207fa08abaff155/Untitled%202.png)

- 모든 샘플에 대해 만족하는 경계를 잡는 것은 어려움 (예를 들면 이상치)
- 그래서 보다 유연한 모델이 필요함
- 이상치를 제거하고 모델을 수립 → 소프트 마진 분류

![SVM%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%20ec2ba239b43c4b10a207fa08abaff155/Untitled%203.png](SVM%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%20ec2ba239b43c4b10a207fa08abaff155/Untitled%203.png)

svm 회귀

```python
# import

np.random.seed(13)
m = 50
X = 2 * np.random.rand(m, 1)
y = (4 + 3 * X + np.random.randn(m, 1)).ravel()

# SVM 회귀 : LinearSVR
from sklearn.svm import LinearSVR

svm_reg = LinearSVR(epsilon=1.5, random_state=13)
svm_reg.fit(X, y)
```

```python
# 입실론 값을 바꿔서 학습

svm_reg1 = LinearSVR(epsilon=1.5, random_state=13)
svm_reg2 = LinearSVR(epsilon=0.5, random_state=13)
svm_reg1.fit(X, y)
svm_reg2.fit(X, y)
```

```python
# 참값과 예측값과 입실론을 비교해서 support vector를 계산하는 함수

def find_support_vectors(svm_reg, X, y):
    y_pred = svm_reg.predict(X)
    off_margin = (np.abs(y - y_pred) >= svm_reg.epsilon)
    return np.argwhere(off_margin)

# support vector까지 그려주는 함수
def plot_svm_regression(svm_reg, X, y, axes):
    x1s = np.linspace(axes[0], axes[1], 100).reshape(100, 1)
    y_pred = svm_reg.predict(x1s)
    plt.plot(x1s, y_pred, "k-", linewidth=2, label=r"$\hat{y}$")
    plt.plot(x1s, y_pred + svm_reg.epsilon, "k--")
    plt.plot(x1s, y_pred - svm_reg.epsilon, "k--")
    plt.scatter(X[svm_reg.support_], y[svm_reg.support_], s=180,
               facecolors='#FFAAAA')
    plt.plot(X, y, "bo")
    plt.xlabel(r"$x_1$", fontsize=18)
    plt.legend(loc="upper left", fontsize=18)
    plt.axis(axes)

# 입실론 밖 support vector 저장
svm_reg1.support_ = find_support_vectors(svm_reg1, X, y)
svm_reg2.support_ = find_support_vectors(svm_reg2, X, y)
```

```python
# 입실론 1.5 설정 결과

plot_svm_regression(svm_reg1, X, y, [0, 2, 3, 11])
plt.title(r"$\epsilon = {}$".format(svm_reg1.epsilon), fontsize=18)
plt.show()
```

![SVM%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%20ec2ba239b43c4b10a207fa08abaff155/Untitled%204.png](SVM%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%20ec2ba239b43c4b10a207fa08abaff155/Untitled%204.png)

```python
# 입실론 0.5 설정 결과
plot_svm_regression(svm_reg2, X, y, [0, 2, 3, 11])
plt.title(r"$\epsilon = {}$".format(svm_reg2.epsilon), fontsize=18)
plt.show()
```

![SVM%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%20ec2ba239b43c4b10a207fa08abaff155/Untitled%205.png](SVM%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%20ec2ba239b43c4b10a207fa08abaff155/Untitled%205.png)

비선형 회귀 테스트를 위한 회귀

```python
m = 100
X = 2 * np.random.rand(m, 1) - 1
y = (0.2 + 0.1 * X + 0.5 * X**2 + np.random.randn(m, 1)/10).ravel()

plt.scatter(X, y);
```

![SVM%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%20ec2ba239b43c4b10a207fa08abaff155/Untitled%206.png](SVM%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%20ec2ba239b43c4b10a207fa08abaff155/Untitled%206.png)

```python
# 비선형 회귀에 svm 적용

from sklearn.svm import SVR

svm_poly_reg1 = SVR(kernel="poly", degree=2, C=100, epsilon=0.1, gamma="scale")
svm_poly_reg2 = SVR(kernel="poly", degree=2, C=0.01, epsilon=0.1, gamma="scale")
svm_poly_reg1.fit(X, y)
svm_poly_reg2.fit(X, y)
```

```python
fig, axes = plt.subplots(ncols=2, figsize=(9, 4), sharey=True)
plt.sca(axes[0])
plot_svm_regression(svm_poly_reg1, X, y, [-1, 1, 0, 1])
plt.title(r"$degree={}, c={}, \epsilon = {}$".format(svm_poly_reg1.degree,
                                                    svm_poly_reg1.C,
                                                    svm_poly_reg1.epsilon),
         fontsize=19)
plt.ylabel(r"$y$", fontsize=18, rotation=0)
plt.sca(axes[1])
plot_svm_regression(svm_poly_reg2, X, y, [-1, 1, 0, 1])
plt.title(r"$degree={}, C={}, \epsilon = {}$".format(svm_poly_reg2.degree,
                                                    svm_poly_reg2.C,
                                                    svm_poly_reg2.epsilon),
          fontsize=18)
plt.show()
```

![SVM%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%20ec2ba239b43c4b10a207fa08abaff155/Untitled%207.png](SVM%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%20ec2ba239b43c4b10a207fa08abaff155/Untitled%207.png)