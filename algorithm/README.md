# 编程题汇总

## 牛客输入处理模版
```Python
import sys
datal = []
for i in sys.stdin:
    v = list(map(int, i.strip().split()))
    datal.append(v)
print(datal)
sys.stdin.readline().strip()
```
