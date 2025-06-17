<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## Matplotlib 繪製直方圖（Histogram）重點整理

### 1. 基本用法

- 使用 `plt.hist(x)` 可快速繪製直方圖，`x` 為數值資料（如隨機樣本）。
- 例如產生 1000 個標準常態分布亂數並繪圖：

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.random.normal(0, 1, 1000)
plt.hist(x)
plt.title('Histogram of 1000 samples from standard normal distribution')
plt.show()
```

    - 預設分成 10 個 bins（箱）。

---

### 2. 主要參數說明

- **x**：輸入資料
- **bins**：箱數或箱邊界（可用整數或 list/array 指定）
- **density**：是否正規化為機率密度（舊版參數為 normed，已棄用）
- **cumulative**：是否繪製累積直方圖
- **histtype**：直方圖型態，如 'bar'（預設）、'step'（線條）
- **其他**：可加顏色、透明度等參數

---

### 3. 自訂 bins 與正規化

- **自訂箱邊界**（如 -5 到 5 間 20 個 bins）：

```python
bins = np.linspace(-5, 5, 21)  # 21個點產生20個bin
plt.hist(x, bins=bins, density=True)
plt.title('Normalized histogram with custom bins')
plt.show()
```

    - `density=True` 使 y 軸為機率密度。

---

### 4. 累積與多種型態

- **累積直方圖**：

```python
plt.hist(x, bins=30, cumulative=True)
```

- **正規化且累積，step 型態**：

```python
plt.hist(x, bins=30, density=True, cumulative=True, histtype='step')
```


---

### 5. 多子圖（subplot）展示多種直方圖

- 可用 `plt.subplot(行, 列, 編號)` 建立子圖，展示不同直方圖型態：

```python
plt.figure(figsize=(10, 8))
plt.subplot(2, 2, 1); plt.hist(x, bins=30)
plt.subplot(2, 2, 2); plt.hist(x, bins=30, density=True)
plt.subplot(2, 2, 3); plt.hist(x, bins=30, cumulative=True)
plt.subplot(2, 2, 4); plt.hist(x, bins=30, density=True, cumulative=True, histtype='step')
plt.tight_layout()
plt.show()
```


---

### 6. 直方圖的回傳值

- `plt.hist()` 會回傳三個值：
    - **n**：每個箱的頻數或密度
    - **bins**：箱邊界
    - **patches**：圖形物件

---

### 7. 其他補充

- **隨機分布來源**：常用 `np.random.normal`（常態分布）、`np.random.gamma`（Gamma 分布）等產生樣本。
- **bins 數量與邊界**：n 個 bins 需 n+1 個邊界點。

---

**總結：**
`plt.hist` 是繪製直方圖的核心工具，支援自訂箱數、正規化、累積、型態與多子圖展示，適合各種資料分布的視覺化需求。

<div style="text-align: center">⁂</div>

