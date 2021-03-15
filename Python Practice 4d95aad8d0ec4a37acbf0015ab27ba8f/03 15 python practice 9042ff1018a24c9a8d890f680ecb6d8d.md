# 03/15 python practice

```python
def plotSinWave(amp, freq, endTime, sampleTime, bias):
    """
    plot sine wave
    y = a sin(2 pi f t) + b
    """
    time = np.arange(0, endTime, sampleTime)
    result = amp * np.sin(2*np.pi*freq*time) + bias
    
    plt.figure(figsize=(12,6))
    plt.plot(time, result); plt.grid(True)
    plt.xlabel('time'); plt.ylabel('sin')
    plt.title(str(amp)+'*sin(2*pi*'+str(freq)+'*t )+'+str(bias))
    plt.show()
```

```python
plotSinWave(2, 1, 10, 0.01, 0)
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