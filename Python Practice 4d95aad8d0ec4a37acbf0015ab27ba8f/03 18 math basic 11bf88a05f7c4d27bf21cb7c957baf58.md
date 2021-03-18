# 03/18 math basic

다항함수

```python
################################## 1

import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(-3, 2, 100)
y = 3*x**2 + 2

plt.figure(figsize=(12,8))
plt.plot(x, y)
plt.xlabel('$x$')
plt.ylabel('$y$')
plt.show()

################################## 2

import matplotlib as mpl

mpl.style.use('seaborn-whitegrid')

plt.figure(figsize=(12,8))
plt.plot(x, y)
plt.xlabel('$x$', fontsize=25)
plt.ylabel('$y$', fontsize=25)
plt.show()
```

![03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled.png](03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled.png)

![03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%201.png](03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%201.png)

다항함수의 x축 방향 이동

```python
x = np.linspace(-5, 5, 100)
y1 = 3*x**2 + 2
y2 = 3*(x+1)**2 + 2
```

```python
plt.figure(figsize=(12,8))
plt.plot(x, y1, lw=2, ls='dashed', label='$y=3x^2 +2$')
plt.plot(x, y2, label='$y=3(x+1)^2 +2$')
plt.legend(fontsize=15)
plt.xlabel('$x$', fontsize=25)
plt.ylabel('$y$', fontsize=25)
plt.show()
```

![03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%202.png](03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%202.png)

지수함수를 파악하기 위한 다양한 경우

```python
x = np.linspace(-2, 2, 100)
a11, a12, a13 = 2, 3, 4
y11, y12, y13 = a11**x, a12**x, a13**x

a21, a22, a23 = 1/2, 1/3, 1/4
y21, y22, y23 = a21**x, a22**x, a23**x
```

```python
fig, ax = plt.subplots(1, 2, figsize=(12,6))

ax[0].plot(x, y11, color='k', label=r"$2^x$")
ax[0].plot(x, y12, '--', color='k', label=r"$3^x$")
ax[0].plot(x, y13, ':', color='k', label=r"$4^x$")
ax[0].legend(fontsize=20)

ax[1].plot(x, y21, color='k', label=r"$(1/2)^x$")
ax[1].plot(x, y22, '--', color='k', label=r"$(1/3)^x$")
ax[1].plot(x, y23, ':', color='k', label=r"$(1/4)^x$")
ax[1].legend(fontsize=20)

plt.show()
```

![03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%203.png](03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%203.png)

지수 증가

```python
plt.figure(figsize=(6,6))
x = np.linspace(0, 10)

plt.plot(x, x**2, '--', color='k', label=r"$x^2$")
plt.plot(x, 2**x, color='k', label=r"$2^x$")

plt.legend(loc="center left", fontsize=25)
plt.xlabel('$x$', fontsize=25)
plt.ylabel('$y$', fontsize=25)

plt.show()
```

![03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%204.png](03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%204.png)

베르누이 분포

![03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%205.png](03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%205.png)

```python
x = np.array([10, 100, 1000, 10000, 100000])
(1+1/x)**x
```

array([2.59374246, 2.70481383, 2.71692393, 2.71814593, 2.71826824])

- 어떤 큰값을 넣어도 2.718281828459045에 수렴하는 ...

로그함수

```python
def log(x, base):
    return np.log(x)/np.log(base)

x1 = np.linspace(0.0001, 5, 1000)
x2 = np.linspace(0.01, 5, 100)

y11, y12 = log(x1, 10), log(x2, np.e)
y21, y22 = log(x1, 1/10), log(x2, 1/np.e)

###### 그리기

fig, ax = plt.subplots(1, 2, figsize=(12,6))

ax[0].plot(x1, y11, label="$\log_{10} x$", color='k')
ax[0].plot(x2, y12, '--', label="$\log_{e} x$", color='k')

ax[0].set_xlabel('$x$', fontsize=25)
ax[0].set_ylabel('$y$', fontsize=25)
ax[0].legend(fontsize=20, loc='lower right')

ax[1].plot(x1, y21, label="\log_{1/10} x$", color='k')
ax[1].plot(x2, y22, '--', label="\log_{1/e} x$", color='k')

ax[1].set_xlabel('$x$', fontsize=25)
ax[1].set_ylabel('$y$', fontsize=25)
ax[1].legend(fontsize=20, loc='upper right')

plt.show()
```

![03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%206.png](03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%206.png)

시그모이드

```python
z = np.linspace(-10, 10, 100)
sigma = 1/(1+np.exp(-z))

plt.figure(figsize=(12,8))
plt.plot(z, sigma)
plt.xlabel('$z$', fontsize=25)
plt.ylabel('$\sigma(z)$', fontsize=25)

plt.show()
```

![03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%207.png](03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%207.png)

다변수 벡터함수

![03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%208.png](03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%208.png)

```python
u = np.linspace(0,1,30)
v = np.linspace(0,1,30)
U, V = np.meshgrid(u, v)
X = U
Y = V
Z = (1+U**2) + (V/(1+V**2))
```

```python
fig = plt.figure(figsize=(7,7))
ax = plt.axes(projection='3d')
ax.xaxis.set_tick_params(labelsize=15)
ax.yaxis.set_tick_params(labelsize=15)
ax.zaxis.set_tick_params(labelsize=15)
ax.set_xlabel('$x$', fontsize=20)
ax.set_ylabel('$y$', fontsize=20)
ax.set_zlabel('$z$', fontsize=20)

ax.scatter3D(X, Y, Z, marker='.', color='gray')

plt.show()
```

![03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%209.png](03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%209.png)

meshgrid

```python
# meshgrid 란?
t = np.linspace(0,4,3)
p = np.linspace(0,4,3)
T, P = np.meshgrid(t, p)
T, P
```

![03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%2010.png](03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%2010.png)

함수의 합성

![03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%2011.png](03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%2011.png)

```python
x = np.linspace(-4, 4, 100)
y = x**3 - 15*x + 30
z = np.log(y)

# 그래프 그리기
fig, ax = plt.subplots(1, 2, figsize=(12,6))

ax[0].plot(x,y, label='$x^3 -15x +30$', color='k')
ax[0].legend(fontsize=18)

ax[1].plot(y,z, label='$\log(y)$', color='k')
ax[1].legend(fontsize=18)

plt.show()
```

![03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%2012.png](03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%2012.png)

```python
### 합성한 것

fig, ax = plt.subplots(1, 2, figsize=(12,6))

ax[0].plot(x,z, '--', label='$\log(f(x))$', color='k')
ax[0].legend(fontsize=18)

ax[1].plot(x,y, label='$x^3 -15x +30$', color='k')
ax[1].legend(fontsize=18)
ax_tmp = ax[1].twinx()
ax_tmp.plot(x,z, '--', label='$\log(f(x))$', color='k')

plt.show()
```

![03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%2013.png](03%2018%20math%20basic%2011bf88a05f7c4d27bf21cb7c957baf58/Untitled%2013.png)