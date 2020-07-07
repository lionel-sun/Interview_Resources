# 编程题汇总

## 牛客输入处理模版
```Python
datal = []
for i in sys.stdin:
    v = list(map(int, i.strip().split()))
    datal.append(v)
print(datal)
```