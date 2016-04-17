# singleton-pattern
单例模式
在整体程序运行过程中，如果某个类只应该有一个实例，那么可通过单例模式来保证。    
- 在python中，可继承Singleton，成为单例
- 更简单的方法：创建模块时，把全局状态放在私有变量中，并提供用于访问此变量的公开函数。例如：
```python
import re
import urllib.request


_URL = "http://www.bankofcanada.ca/stats/assets/csv/fx-seven-day.csv"


def get(refresh=False):
    if refresh:
        get.rates = {}
    if get.rates:
        return get.rates
    with urllib.request.urlopen(_URL) as file:
        for line in file:
            line = line.rstrip().decode("utf-8")
            if not line or line.startswith(("#", "Date")):
                continue
            name, currency, *rest = re.split(r"\s*,\s*", line)
            key = "{} ({})".format(name, currency)
            try:
                get.rates[key] = float(rest[-1])
            except ValueError as err:
                print("error {}: {}".format(err, line))
    return get.rates
get.rates = {} ＃创建保存私有数据的rates字典，并将其设置为get()函数的属性
```

