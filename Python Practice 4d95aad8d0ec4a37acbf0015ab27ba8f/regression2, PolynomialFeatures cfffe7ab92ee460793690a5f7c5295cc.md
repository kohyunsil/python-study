# regression2, PolynomialFeatures

```python
plt.plot(X, y, "b.")
plt.xlabel("$x_1$",fontsize=18)
plt.ylabel("$y$", rotation=0, fontsize=18)
plt.axis([-3, 3, 0, 10])
plt.show()
```

![regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled.png](regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled.png)

```python
from sklearn.preprocessing import PolynomialFeatures

poly_features = PolynomialFeatures(degree=2, include_bias=False)
X_poly = poly_features.fit_transform(X)
```

```python
from sklearn.linear_model import LinearRegression

lin_reg = LinearRegression()
lin_reg.fit(X_poly, y)
lin_reg.intercept_, lin_reg.coef_
```

![regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%201.png](regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%201.png)

```python
X_new = np.linspace(-3, 3, 100).reshape(100, 1)
X_new_poly = poly_features.transform(X_new)
y_new = lin_reg.predict(X_new_poly)
```

```python
plt.plot(X, y, "b.")
plt.plot(X_new, y_new, "r-", linewidth=2, label="Predictions")
plt.xlabel("$x_1$",fontsize=18)
plt.ylabel("$y$", rotation=0, fontsize=18)
plt.legend(loc="upper left", fontsize=14)
plt.axis([-3, 3, 0, 10])
plt.show()
```

![regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%202.png](regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%202.png)

```python
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import Pipeline

for style, width, degree in (("g-", 1, 300), ("b--", 2, 2), ("r-+", 2, 1)):
    polybig_features = PolynomialFeatures(degree=degree, include_bias=False)
    std_scaler = StandardScaler()
    lin_reg = LinearRegression()
    polynomial_regression = Pipeline([
        ("poly_Features", polybig_features),
        ("std_scaler", std_scaler),
        ("lin_reg", lin_reg),
    ])
    polynomial_regression.fit(X, y)
    y_newbig = polynomial_regression.predict(X_new)
    plt.plot(X_new, y_newbig, style, label=str(degree), linewidth=width)

plt.plot(X, y, "b.", linewidth=3)
plt.legend(loc="upper left")
plt.xlabel("$x_1$",fontsize=18)
plt.ylabel("$y$", rotation=0, fontsize=18)
plt.axis([-3, 3, 0, 10])
plt.show()
```

![regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%203.png](regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%203.png)

```python
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split

def plot_learning_curves(model, X, y):
    X_train, X_val, y_train, y_val = train_test_split(X, y, test_size=0.2,
                                                     random_state=5)
    train_errors, val_errors = [], []
    for m in range(1, len(X_train)):
        model.fit(X_train[:m], y_train[:m])
        y_train_predict = model.predict(X_train[:m])
        y_val_predict = model.predict(X_val)
        train_errors.append(mean_squared_error(y_train[:m], y_train_predict))
        val_errors.append(mean_squared_error(y_val, y_val_predict))
        
    plt.plot(np.sqrt(train_errors), "r-+", linewidth=2, label="train")
    plt.plot(np.sqrt(val_errors), "b-", linewidth=3, label="val")
    plt.legend(loc="upper right", fontsize=14)
    plt.xlabel("Training set size",fontsize=14)
    plt.ylabel("RMSE", fontsize=14)

###############################

lin_reg = LinearRegression()
plot_learning_curves(lin_reg, X, y)
plt.axis([0, 80, 0, 3])
plt.show()
```

![regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%204.png](regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%204.png)

```python
from sklearn.pipeline import Pipeline

polynomial_regression = Pipeline([
    ("poly_Features", PolynomialFeatures(degree=10, include_bias=False)),
    ("lin_reg", LinearRegression()),
])

plot_learning_curves(polynomial_regression, X, y)
plt.axis([0, 80, 0, 3])
plt.show()
```

![regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%205.png](regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%205.png)

```python
from sklearn.datasets import make_moons
X, y = make_moons(n_samples=100, noise=0.15, random_state=13)

X.shape, y.shape

def plot_dataset(X, y, axes):
    plt.plot(X[:, 0][y==0], X[:, 1][y==0], "bs")
    plt.plot(X[:, 0][y==1], X[:, 1][y==1], "g^")
    plt.axis(axes)
    plt.grid(True, which='both')
    plt.xlabel(r"$x_1$", fontsize=20)
    plt.ylabel(r"$x_2$", fontsize=20, rotation=0)

plot_dataset(X, y, [-1.5, 2.5, -1, 1.5])
plt.show()
```

![regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%206.png](regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%206.png)

```python
polynomial_svm_clf = Pipeline([
    ("scaler", StandardScaler()),
    ("svm_clf", LinearSVC(C=10, loss="hinge", random_state=13))
])
polynomial_svm_clf.fit(X, y)

def plot_predictions(clf, axes):
    x0s = np.linspace(axes[0], axes[1], 100)
    x1s = np.linspace(axes[2], axes[3], 100)  
    x0, x1 = np.meshgrid(x0s, x1s)
    X = np.c_[x0.ravel(), x1.ravel()]
    y_pred = clf.predict(X).reshape(x0.shape)
    y_decision = clf.decision_function(X).reshape(x0.shape)
    plt.contourf(x0, x1, y_pred, cmap=plt.cm.brg, alpha=0.2)
    plt.contourf(x0, x1, y_decision, cmap=plt.cm.brg, alpha=0.1)

plot_predictions(polynomial_svm_clf, [-1.5, 2.5, -1, 1.5])
plot_dataset(X, y, [-1.5, 2.5, -1, 1.5])
plt.show()
```

![regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%207.png](regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%207.png)

```python
polynomial_svm_clf = Pipeline([
    ("poly_features", PolynomialFeatures(degree=3)),
    ("scaler", StandardScaler()),
    ("svm_clf", LinearSVC(C=10, loss="hinge", random_state=13))
])
polynomial_svm_clf.fit(X, y)

plot_predictions(polynomial_svm_clf, [-1.5, 2.5, -1, 1.5])
plot_dataset(X, y, [-1.5, 2.5, -1, 1.5])
plt.show()
```

![regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%208.png](regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%208.png)

```python
from sklearn.svm import SVC

poly_kernel_svm_clf = Pipeline([
    ("scaler", StandardScaler()),
    ("svm_clf", SVC(kernel="poly", degree=3,  coef0=1, C=5))
])
poly_kernel_svm_clf.fit(X, y)

plot_predictions(polynomial_svm_clf, [-1.5, 2.5, -1, 1.5])
plot_dataset(X, y, [-1.5, 2.5, -1, 1.5])
plt.show()
```

![regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%209.png](regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%209.png)

```python
rbf_kernel_svm_clf = Pipeline([
    ("scaler", StandardScaler()),
    ("svm_Clf", SVC(kernel="rbf", gamma=5, C=0.001))
])
rbf_kernel_svm_clf.fit(X, y)

plot_predictions(rbf_kernel_svm_clf, [-1.5, 2.5, -1, 1.5])
plot_dataset(X, y, [-1.5, 2.5, -1, 1.5])
plt.show()
```

![regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%2010.png](regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%2010.png)

```python
rbf_kernel_svm_clf = Pipeline([
    ("scaler", StandardScaler()),
    ("svm_clf", SVC(kernel="rbf", gamma=5, C=1000))
])
rbf_kernel_svm_clf.fit(X, y)

plot_predictions(rbf_kernel_svm_clf, [-1.5, 2.5, -1, 1.5])
plot_dataset(X, y, [-1.5, 2.5, -1, 1.5])
plt.show()
```

![regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%2011.png](regression2,%20PolynomialFeatures%20cfffe7ab92ee460793690a5f7c5295cc/Untitled%2011.png)