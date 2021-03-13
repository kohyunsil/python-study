# 03/13 Pandas_DataFrame_inputoutput

pandas DataFrame input output
- csv, excel

```python
df = pd.DataFrame({"name": list("ABC"), "age": range(20, 23)})
df
```

```python
df.to_csv("test.csv", index=False)

pd.read_csv("test.csv")
```

csv 파일 저장시 데이터에 ,가 있으면 에러가 발생 가능
tsv 파일 사용

```python
df.to_csv("test.tsv", index=False, sep="\t")
```

excel로 데이터 저장, 불러오기

```python
df.to_excel("test.xlsx", sheet_name="test", engine="xlsxwriter", encoding="utf-8-sig")

!ls

pd.read_excel("test.xlsx", "test")
```