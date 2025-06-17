<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## Matplotlib 的對數座標圖（Logarithmic Plots）整理

### 1. 為什麼要用對數座標圖？

- 對數座標圖（log plot）可將冪次關係（如 y = x^α）轉換為直線，方便判斷資料的趨勢與指數。
- 在 log-log 圖上，斜率即為冪次 α。
- 常用於資料跨數量級、成長快速、或有冪次關係的情境。

---

### 2. 常用的對數繪圖函式

| 函式 | x 軸 | y 軸 | 用途說明 |
| :-- | :-- | :-- | :-- |
| plt.plot | 線性 | 線性 | 一般線性圖 |
| plt.semilogx | **對數** | 線性 | x 軸對數、y 軸線性 |
| plt.semilogy | 線性 | **對數** | x 軸線性、y 軸對數 |
| plt.loglog | **對數** | **對數** | x、y 軸皆對數（最常用於冪次關係） |


---

### 3. 基本範例

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.logspace(-1, 1, 40)  # 0.1 ~ 10，對數等距
y1 = x ** 2
y2 = x ** 0.5

plt.loglog(x, y1, 'bo-', label="y = x^2")
plt.loglog(x, y2, 'gs-', label="y = x^0.5")
plt.xlabel("x (log scale)")
plt.ylabel("y (log scale)")
plt.legend()
plt.show()
```

- `np.logspace(-1, 1, 40)` 產生 0.1 到 10 的對數等距 x 值。
- `plt.loglog()` 會將 x、y 軸都設為對數尺度。

---

### 4. 對數圖的數學意義

- 對於 y = x^α，取 log 變成 log(y) = α·log(x)，即在 log-log 圖上為一條斜率為 α 的直線。
- 這讓你可以直觀判斷資料的冪次關係與成長速率。

---

### 5. 注意事項

- 預設對數底數為 10，可用 `plt.xscale("log", base=2)` 改為 2 為底。
- 若 x 或 y 有 0 或負數，對數圖會報錯（log(0) 無定義）。
- 若要均勻分布在對數尺度上，請用 `np.logspace` 產生 x 值。

---

### 6. 小結

- `semilogx()`：x 軸對數、y 軸線性
- `semilogy()`：y 軸對數、x 軸線性
- `loglog()`：x、y 軸皆對數
- 對數圖適合資料跨數量級或冪次關係的視覺化
- `np.logspace()` 可產生對數等距的 x 值

---

**建議練習：**
試著將常見的指數成長、冪次關係資料用 loglog() 畫圖，體會對數圖的威力！

