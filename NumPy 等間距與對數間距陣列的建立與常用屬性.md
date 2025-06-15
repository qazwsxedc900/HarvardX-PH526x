<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## NumPy 等間距與對數間距陣列的建立與常用屬性

### 一、建立等間距陣列：`np.linspace`

- **功能**：產生一個在指定區間內，等間距分布的數值陣列。
- **語法**：

```python
np.linspace(start, stop, num)
```

    - `start`：起始值
    - `stop`：結束值（**包含**在內）
    - `num`：元素數量
- **範例**：產生 10 個從 0 到 100 的等間距數值

```python
import numpy as np
arr = np.linspace(0, 100, 10)
print(arr)
# 輸出: [  0.  11.111...  22.222... ... 100.]
```

    - `np.linspace()` 預設會**包含**終點【1】【2】【3】【4】【5】【6】。

---

### 二、建立對數間距陣列：`np.logspace`

- **功能**：產生一個在對數尺度下等間距分布的數值陣列。
- **語法**：

```python
np.logspace(start_exp, stop_exp, num)
```

    - `start_exp`：起始指數（以 10 為底）
    - `stop_exp`：結束指數（以 10 為底）
    - `num`：元素數量
- **範例**：產生 10 個從 10 到 100 的對數間距數值

```python
arr = np.logspace(1, 2, 10)  # 10^1 到 10^2
print(arr)
# 輸出: [ 10.          12.91549665 ... 100.        ]
```

- 若要產生 250 到 500 的對數間距陣列，需先取 log10：

```python
arr = np.logspace(np.log10(250), np.log10(500), 10)
print(arr)
```


---

### 三、常用屬性：shape 與 size

- **shape**：回傳陣列的形狀（各維度大小），是屬性不是方法

```python
x = np.array([[1,2,3],[4,5,6]])
print(x.shape)  # (2, 3)
```

- **size**：回傳陣列元素總數，也是屬性

```python
print(x.size)   # 6
```


---

### 四、條件判斷：`np.any` 與 `np.all`

- **np.any**：判斷陣列中是否有任一元素符合條件

```python
x = np.random.rand(10)  # 0~1 間 10 個亂數
print(np.any(x > 0.9))  # 只要有一個大於 0.9 就回傳 True
```

- **np.all**：判斷陣列中是否所有元素都符合條件

```python
print(np.all(x >= 0.1))  # 所有元素都大於等於 0.1 才會回傳 True
```

- 這兩個函式回傳單一布林值（True/False），代表全陣列的判斷結果。

---

### 五、重點總結

- `np.linspace` 產生**等間距**數列，包含終點。
- `np.logspace` 產生**對數間距**數列，參數需給對數值。
- `shape` 與 `size` 是陣列屬性，可查詢結構與元素數。
- `np.any`、`np.all` 可快速判斷陣列是否有元素或全部元素符合條件。

---

這些功能是科學運算、數值分析與資料視覺化時常用的基礎工具。

<div style="text-align: center">⁂</div>

[^1]: https://numpy.org/doc/2.1/reference/generated/numpy.linspace.html

[^2]: https://www.datacamp.com/tutorial/how-to-use-the-numpy-linspace-function

[^3]: https://realpython.com/np-linspace-numpy/

[^4]: https://labex.io/tutorials/numpy-generating-evenly-spaced-numbers-with-numpy-86473

[^5]: https://fritz.ai/exploring-numpys-linspace-function/

[^6]: https://www.youtube.com/watch?v=BApUYqiMBfk

[^7]: https://stackoverflow.com/questions/24890558/using-python-numpy-linspace-in-for-loop

