# linear_regression

- model 1

```python
X = final.drop('worldwide_gross_income',1)
y = final['worldwide_gross_income']

X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=13)
```

```python
reg = LinearRegression()
reg.fit(X_train, y_train)
```

```python
pred_tr = reg.predict(X_train)
pred_test = reg.predict(X_test)
rmse_tr = (np.sqrt(mean_squared_error(y_train, pred_tr)))
rmse_test = (np.sqrt(mean_squared_error(y_test, pred_test)))

print('RMSE of Train Data : ', round(rmse_tr, 3))
print('RMSE of Test Datea : ', round(rmse_test,3))
```

![linear_regression%208439ec50d50443fb8a110793a1dfd74a/Untitled.png](linear_regression%208439ec50d50443fb8a110793a1dfd74a/Untitled.png)

```python
predicted = reg.predict(X_test)

print('rmean_squared_errors: {}'.format(round(np.sqrt(mean_squared_error(y_test, predicted)),3)))
print('r2_score: {}'.format(round(r2_score(y_test, predicted),6)))
```

![linear_regression%208439ec50d50443fb8a110793a1dfd74a/Untitled%201.png](linear_regression%208439ec50d50443fb8a110793a1dfd74a/Untitled%201.png)

```python
plt.figure(figsize=(12,8))
plt.scatter(y_test, pred_test)
plt.xlabel("Actual")
plt.ylabel("Predicted")
plt.title("Real vs Predicted")
plt.plot([0,2.5e9], [0,2.5e9], 'r')
plt.show()
```

![linear_regression%208439ec50d50443fb8a110793a1dfd74a/Untitled%202.png](linear_regression%208439ec50d50443fb8a110793a1dfd74a/Untitled%202.png)

```python
lm = sm.OLS(y_train, X_train).fit()
lm.summary()
```

![linear_regression%208439ec50d50443fb8a110793a1dfd74a/Untitled%203.png](linear_regression%208439ec50d50443fb8a110793a1dfd74a/Untitled%203.png)