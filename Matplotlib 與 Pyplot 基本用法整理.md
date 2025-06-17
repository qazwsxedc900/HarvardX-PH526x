<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## Matplotlib 與 Pyplot 基本用法整理

### 1. 什麼是 Matplotlib 與 Pyplot？

- **Matplotlib** 是 Python 最常用的繪圖函式庫，可產生出版等級的圖表。
- **Pyplot** 是 Matplotlib 的子模組，提供類似 Matlab 的簡易繪圖介面，常用於互動式資料探索與視覺化。
- 幾乎所有人都這樣匯入：

```python
import matplotlib.pyplot as plt
```


---

### 2. Pyplot 的基本操作流程

- **狀態機機制**：Pyplot 會自動管理「目前的圖形」和「目前的座標軸」。
- **常見資料型態**：通常用 NumPy array 存資料，也可用 Python list。

---

### 3. 基本繪圖指令

#### (1) 單一序列繪圖

```python
plt.plot([0, 1, 4, 9, 16])
plt.show()  # 在一般 Python Shell 需加這行才會顯示圖形
```

- 只給一個序列，預設 x 軸為索引（0, 1, 2, ...）。


#### (2) x, y 雙序列繪圖

```python
import numpy as np
x = np.linspace(0, 10, 20)
y = x ** 2
plt.plot(x, y)
plt.show()
```

- 第一個參數為 x，第二個為 y。


#### (3) 格式化字串控制顏色、線型、標記

```python
plt.plot(x, y, 'bo-')  # 藍色、圓點、實線
```

- 'b'：blue，'g'：green，'o'：circle，'s'：square，'-'：實線。


#### (4) 關鍵字參數（Keyword Arguments）

```python
plt.plot(x, y, 'bo-', linewidth=2, markersize=8)
```

- `linewidth`：線寬
- `markersize`：點的大小

---

### 4. 多組資料繪圖

```python
y2 = x ** 1.5
plt.plot(x, y, 'bo-', linewidth=2, markersize=8)
plt.plot(x, y2, 'gs-', linewidth=2, markersize=8)
plt.show()
```

- 可多次呼叫 `plot()` 疊加多條線。

---

### 5. 互動模式與顯示

- 在 **IPython/Jupyter**，執行 `plt.plot(...)` 就會自動顯示圖形。
- 在 **一般 Python Shell**，需加 `plt.show()` 才會顯示圖形。

---

### 6. 小技巧

- 用分號 `;` 可抑制 IPython/Jupyter 的物件輸出，只顯示圖形。
- 可用 `plt.xlabel()`、`plt.ylabel()`、`plt.title()`、`plt.legend()` 增加標籤與圖例。

---

### 7. 常見格式化字串（Matlab 風格）

| 顏色 | 符號 | 意義 |
| :-- | :-- | :-- |
| 'b' | 藍色 | blue |
| 'g' | 綠色 | green |
| 'r' | 紅色 | red |
| 'k' | 黑色 | black |
| 'o' | 圓點 | circle |
| 's' | 方塊 | square |
| '-' | 實線 | solid |
| '--' | 虛線 | dashed |


---

### 8. 總結

- Matplotlib 是 Python 最強大的繪圖工具，Pyplot 讓繪圖變得簡單直觀。
- 基本流程：`plt.plot(...)` → `plt.show()`
- 支援多樣格式、標記、線型與關鍵字參數，適合各種資料視覺化需求。

---

**建議：**
初學者可多練習用 Pyplot 快速畫圖，熟悉後再深入學習 Matplotlib 進階功能！

