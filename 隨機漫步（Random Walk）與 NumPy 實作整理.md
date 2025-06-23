<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## 隨機漫步（Random Walk）與 NumPy 實作整理

### 一、隨機漫步的概念

- 隨機漫步是一種重要的隨機過程，可用於模擬分子運動、動物移動、GPS 追蹤等各種現象。
- 基本原理：每一步的位置等於前一步的位置加上一個隨機步長（displacement）。
- 數學表示：
    - 初始位置 \$ x_0 \$
    - 第 \$ t \$ 步位置：\$ x_t = x_{t-1} + \Delta x_t \$
    - 一般化：\$ x_k = x_0 + \sum_{i=1}^{k} \Delta x_i \$

---

### 二、NumPy 隨機漫步的實作流程

1. **產生隨機步長**
    - 用 `np.random.normal(均值, 標準差, (2, 步數))` 產生二維（x, y）隨機步長陣列。
    - 例如：`delta_x = np.random.normal(0, 1, (2, 5))` 產生 5 步，每步有 x, y 兩個分量。
2. **累積步長得到位置**
    - 用 `np.cumsum(delta_x, axis=1)` 計算每一步的位置（累積和）。
    - 例如：`X = np.cumsum(delta_x, axis=1)`
3. **加上起點（原點）**
    - 用 `np.concatenate` 把起點  加到最前面，確保從原點出發。
    - 例如：

```python
X0 = np.zeros((2, 1))
X = np.concatenate((X0, np.cumsum(delta_x, axis=1)), axis=1)
```

4. **繪圖**
    - 用 `plt.plot(X, X[^1], 'ro-')` 畫出路徑（紅色圓點、線段）。
    - 可用 `plt.savefig('rw.pdf')` 儲存圖檔。

---

### 三、範例程式碼（5 步隨機漫步）

```python
import numpy as np
import matplotlib.pyplot as plt

# 產生隨機步長
delta_x = np.random.normal(0, 1, (2, 5))

# 累積步長並加上原點
X0 = np.zeros((2, 1))
X = np.concatenate((X0, np.cumsum(delta_x, axis=1)), axis=1)

# 繪圖
plt.plot(X[^0], X[^1], 'ro-')
plt.title("Random Walk (5 steps)")
plt.savefig('rw.pdf')
plt.show()
```


---

### 四、延伸應用

- **多步/多次模擬**：將步數改為 100 或 10,000，觀察路徑變化。
- **高維度**：可擴展到三維或更高維度。
- **不同分布**：可將步長分布改為其他隨機分布（如均勻分布）。

---

### 五、NumPy 常用技巧

- `np.cumsum(a, axis)`：計算累積和。
- `np.concatenate((a, b), axis)`：沿指定軸串接陣列。
- `np.random.normal(loc, scale, size)`：產生常態分布亂數。

---

### 六、重點整理

- 隨機漫步本質是「累積隨機步長」。
- NumPy 提供高效的亂數產生、累積和與陣列操作工具，讓隨機漫步模擬簡單又快速。
- 實作時，記得加上起點，並可用 matplotlib 畫出路徑。

---

這些就是隨機漫步的數學邏輯、NumPy 實作步驟，以及如何將結果視覺化的完整整理。

<div style="text-align: center">⁂</div>

[^1]: paste.txt

