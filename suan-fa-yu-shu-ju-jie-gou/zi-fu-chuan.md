# String

## 字符串匹配

### 朴素的串匹配算法

1. 从左到右逐个字符匹配
2. 发现不匹配时转去考虑目标串里的下一个位置是否与模式串匹配

![&#x6734;&#x7D20;&#x7684;&#x5B57;&#x7B26;&#x4E32;&#x5339;&#x914D;](../.gitbook/assets/image%20%2820%29.png)

```python
def naive_matching(t, p):
    m, n = len(p), len(t)
    i, j = 0, 0
    while i < m and j < n:  # i == m 说明找到匹配
        if p[i] == t[j]:  # 字符串相同：考虑下一对字符串
            i, j = i + 1, j + 1
        else:  # 字符不同：考虑t中下一个位置
            i, j = 0, j - i + 1
    if i == m:  # 找到匹配， 返回其开始下标
        return j - i
    return -1  # 无匹配，返回特殊值
```

### 无回溯串匹配算法（KMP算法）

![&#x6734;&#x7D20;&#x5339;&#x914D;&#x548C;KMP&#x5339;&#x914D;&#x8FC7;&#x7A0B;](../.gitbook/assets/image%20%2812%29.png)

| 字串 | a | g | c | t | a | g | c | a | g | c | t | a | g | c | t | g |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| pnext | -1 | 0 | 0 | 0 | 0 | 1 | 2 | 3 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 4 |

```python
def generate_pnext(p):
    i, k, m = 0, -1, len(p)
    pnext = [-1] * m
    while i < m - 1:  # 生成下一个pnext元素值
        if k == -1 or p[i] == p[k]:
             i, k = i + 1, k + 1
             if p[i] == p[k]:
                 pnext[i] = pnext[k]
             else:
                 pnext[i] = k  # 设置pnext元素
         else:
             k = pnext[k]  # 退到更短相同前缀
     return pnext
```

```python
def matching_KMP(t, p, pnext):
    n, m = len(t), len(p)
    j, i = 0, 0
    while j < n and i < m:  # i == m 说明找到了匹配
        if i == -1 or t[j] == p[i]:  # 考虑p中下一个字符
            j, i = j + 1, i + 1
        else:  # 失败：考虑pnext决定下一个字符
            i = pnext[i]
    if i == m:  # 找到匹配，返回其下标
        return j - i
    return -1  # 无匹配，返回特殊值
```



