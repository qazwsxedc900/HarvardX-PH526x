<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## 用 NumPy 產生隨機數與統計模擬的整理

### 一、NumPy 隨機數產生的優點

- **支援多種隨機分布**，功能豐富。
- **速度快**，特別適合大量資料的隨機模擬。

---

### 二、常見隨機分布的產生方式

#### 1. 標準均勻分布（0~1 間均勻分布）

- 產生單一亂數：

```python
np.random.random()
```

- 產生一維陣列（如 5 個亂數）：

```python
np.random.random(5)
```

- 產生二維陣列（如 5x3）：

```python
np.random.random((5, 3))
```


#### 2. 常態分布（正態分布）

- 產生單一標準常態亂數（均值0，標準差1）：

```python
np.random.normal(0, 1)
```

- 產生一維陣列（5個）：

```python
np.random.normal(0, 1, 5)
```

- 產生二維陣列（2x5）：

```python
np.random.normal(0, 1, (2, 5))
```


---

### 三、產生隨機整數（骰子模擬）

- 用 `np.random.randint` 產生指定範圍的隨機整數。
- 例如，產生 10 列 3 行，每個元素為 1~6 的隨機整數（骰子點數）：

```python
x = np.random.randint(1, 7, (10, 3))
print(x.shape)  # (10, 3)
```


---

### 四、資料加總與 np.sum 的用法

- `np.sum(x)`：將所有元素加總。
- `np.sum(x, axis=0)`：對每一「欄」加總（跨列）。
- `np.sum(x, axis=1)`：對每一「列」加總（跨欄），常用於統計每次模擬的總和。

---

### 五、骰子總和分布的模擬流程

1. **產生隨機骰子表格**
例如要模擬 10000 次，每次擲 10 顆骰子：

```python
x = np.random.randint(1, 7, (10000, 10))
```

2. **計算每次的總和**

```python
y = np.sum(x, axis=1)
```

    - `y` 是一維陣列，長度 10000，代表每次 10 顆骰子的總和。
3. **繪製直方圖**

```python
import matplotlib.pyplot as plt
plt.hist(y)
plt.show()
```


- 若將模擬次數從 100 增加到 10,000 或 1,000,000，直方圖會越來越平滑，呈現鐘型分布（常態分布），這是中央極限定理的體現。

---

### 六、NumPy 的效能優勢

- 用 NumPy 寫同樣的模擬程式，**速度比純 Python 迴圈快 10 倍以上**，且程式碼更簡潔。
- 適合大量資料與科學運算。

---

### 七、總結

- NumPy 可快速產生各種隨機分布的亂數，支援一維、二維甚至多維陣列。
- `np.random.randint` 適合模擬骰子等離散隨機事件。
- `np.sum` 可彈性指定加總方向，方便進行統計計算。
- NumPy 讓大規模隨機模擬變得高效且容易。

<div style="text-align: center">⁂</div>

[^1]: paste.txt

