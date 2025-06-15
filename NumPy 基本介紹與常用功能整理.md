<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## NumPy 基本介紹與常用功能整理

### 什麼是 NumPy？

NumPy（Numerical Python）是 Python 用於科學與數值運算的核心模組，提供高效能的 n 維陣列（ndarray）資料結構，以及大量的數學、線性代數、隨機數生成等工具[^5][^2]。

---

### NumPy 陣列的特點

- **固定大小**：建立後大小不可變。
- **元素型別一致**：所有元素必須為相同資料型別，預設為浮點數。
- **高效運算**：比 Python list 更快且省記憶體。
- **支援多維（n 維）**：可當作向量、矩陣或更高維資料結構[^5][^2]。

---

### 基本用法

#### 1. 匯入 NumPy

```python
import numpy as np
```

這是最常見的匯入方式[^5][^2]。

#### 2. 建立陣列

- **由 list 或 tuple 建立**

```python
arr = np.array([1, 2, 3, 4, 5])
arr2 = np.array((1, 2, 3, 4, 5))
```

產生一個 ndarray 物件[^2][^5][^6]。
- **建立全為 0 或 1 的陣列**

```python
zero_vector = np.zeros(5)        # 一維，5 個元素全為 0
zero_matrix = np.zeros((5, 3))   # 二維，5 列 3 行全為 0
ones_vector = np.ones(5)         # 一維，5 個元素全為 1
ones_matrix = np.ones((2, 4))    # 二維，2 列 4 行全為 1
```

`np.zeros` 和 `np.ones` 都可接受 shape 參數（整數或 tuple）[^5][^2][^8]。
- **建立未初始化的陣列**

```python
empty_arr = np.empty((2, 3))  # 內容未定，速度快但需小心使用
```

- **由既定數值建立**

```python
x = np.array([1, 2, 3])
y = np.array([2, 4, 6])
```

- **多維陣列（矩陣）**

```python
A = np.array([[1, 3], [5, 9]])  # 2x2 矩陣
```


#### 3. 常用特殊陣列

- **arange**

```python
arr = np.arange(0, 8)  # 產生 [0, 1, 2, ..., 7]
```

- **linspace**

```python
arr = np.linspace(0, 1, 5)  # 產生 0~1 間等距的 5 個數
```


---

### 陣列操作

#### 1. 基本運算（加減乘除）

NumPy 支援陣列間的元素級運算[^1][^5][^9]：

```python
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

a + b    # array([5, 7, 9])
a - b    # array([-3, -3, -3])
a * b    # array([4, 10, 18])
a / b    # array([0.25, 0.4, 0.5])
```


#### 2. 求和、最大最小值

```python
a.sum()         # 總和
a.max()         # 最大值
a.min()         # 最小值
```

- **二維陣列指定軸求和**

```python
B = np.array([[1, 1], [2, 2]])
B.sum(axis=0)  # 列加總：[3, 3]
B.sum(axis=1)  # 行加總：[2, 4]
```


#### 3. 轉置（transpose）

```python
A = np.array([[1, 3], [5, 9]])
A_T = A.transpose()  # 或 A.T
# 結果：[[1, 5], [3, 9]]
```

轉置會將行與列對調[^3][^5]。

#### 4. 形狀變換與攤平

- **reshape**

```python
arr = np.arange(6)
arr2d = arr.reshape((2, 3))  # 變成 2x3
```

- **攤平成一維**

```python
arr.ravel()  # 攤平成一維
```


---

### 進階功能

- **廣播（broadcasting）**：不同形狀的陣列可自動對齊運算[^1][^5]。
- **索引與切片**：類似 Python list，但更強大，支援多維索引[^8]。
- **條件選取**：可用布林陣列快速選取元素[^8]。
- **與 C/C++/Fortran 整合**：適合高效能科學運算[^5]。

---

### 小結

NumPy 提供高效能、多維陣列與豐富的數值運算工具，是 Python 科學計算的基礎。常見功能包括陣列建立、形狀變換、基本運算、轉置、索引切片與廣播等，能大幅提升數據處理效率[^5][^2][^8]。

<div style="text-align: center">⁂</div>

[^1]: https://numpy.org/devdocs/user/absolute_beginners.html

[^2]: https://www.w3schools.com/Python/numpy/numpy_creating_arrays.asp

[^3]: https://scipy-lectures.org/intro/numpy/operations.html

[^4]: https://www.youtube.com/watch?v=lLRBYKwP8GQ

[^5]: https://numpy.org/doc/stable/user/absolute_beginners.html

[^6]: https://www.datacamp.com/tutorial/python-numpy-tutorial

[^7]: https://data-flair.training/blogs/numpy-array/

[^8]: https://www.pythontutorial.net/python-numpy/

[^9]: https://www.programiz.com/python-programming/numpy/basic-array-operations

[^10]: https://www.w3schools.com/python/numpy/default.asp

[^11]: https://numpy.org/devdocs/user/quickstart.html

[^12]: https://www.youtube.com/watch?v=a8aDcLk4vRc

[^13]: https://codesignal.com/learn/courses/numpy-basics/lessons/basic-array-operations-in-numpy

[^14]: https://jakevdp.github.io/PythonDataScienceHandbook/02.02-the-basics-of-numpy-arrays.html

