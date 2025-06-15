<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## NumPy 陣列的進階索引與布林索引整理

### 一、陣列索引（整數陣列/列表索引）

- NumPy 陣列可以用**其他陣列**或**list**來當作索引，這稱為「進階索引」。
- 例如：

```python
import numpy as np
z1 = np.array([1, 3, 5, 7, 9])
ind = [0, 2, 3]  # 也可以是 np.array([0, 2, 3])
print(z1[ind])   # 輸出 [1, 5, 7]
```

- 這樣可以一次選出多個指定位置的元素。

---

### 二、布林（邏輯）索引

- 可以用布林陣列（True/False 組成）來選取元素。
- 例如：

```python
z1 = np.array([1, 3, 5, 7, 9])
mask = z1 > 6  # 得到 array([False, False, False, True, True])
print(z1[mask])  # 輸出 [7, 9]
```

- 也可以直接寫成：

```python
print(z1[z1 > 6])  # 輸出 [7, 9]
```

- 布林索引也可用來選取另一個陣列的對應元素：

```python
z2 = z1 + 1  # array([2, 4, 6, 8, 10])
print(z2[z1 > 6])  # 輸出 [8, 10]
```


---

### 三、索引與切片的差異：**copy** vs **view**

- **切片（slice）**：回傳的是原陣列的「view」，修改切片內容會影響原陣列。

```python
w = z1[0:3]  # 取前3個元素
w[0] = 100
print(z1)  # z1[0] 也會被改變
```

- **進階索引（array/list/布林索引）**：回傳的是「copy」，修改 copy 不會影響原陣列。

```python
idx = [0, 1, 2]
w = z1[idx]
w[0] = 100
print(z1)  # z1 不會被改變
```


---

### 四、重點總結

1. NumPy 支援用 list、ndarray 或布林陣列作為索引，能夠高效選取多個元素。
2. 布林索引可用於條件篩選，非常直覺且常用。
3. **切片**回傳 view，**進階索引**回傳 copy。這點在需要修改資料時非常重要，避免不小心改到原始資料。
4. 進階索引與切片可混用於多維陣列。

---

### 五、簡單範例總覽

```python
import numpy as np
z1 = np.array([1, 3, 5, 7, 9])
z2 = z1 + 1

# 整數陣列索引
idx = [0, 2, 3]
print(z1[idx])  # [1, 5, 7]

# 布林索引
print(z1[z1 > 6])  # [7, 9]
print(z2[z1 > 6])  # [8, 10]

# 切片是 view
w = z1[0:3]
w[0] = 100
print(z1)  # z1[0] 也會變成 100

# 進階索引是 copy
w = z1[[0, 1, 2]]
w[0] = 3
print(z1)  # z1 不受影響
```


---

這些技巧是處理 NumPy 陣列時非常實用的基礎，能讓你高效又安全地選取、修改與分析資料。

