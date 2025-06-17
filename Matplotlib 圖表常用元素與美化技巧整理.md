<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## Matplotlib 圖表常用元素與美化技巧整理

### 1. 加入座標軸標籤（xlabel, ylabel）

- 設定 x 軸與 y 軸的標籤：

```python
plt.xlabel("X")
plt.ylabel("Y")
```

- 支援 LaTeX 語法（加 `$` 符號），可顯示數學符號：

```python
plt.xlabel("$x$")
plt.ylabel("$y$")
```


---

### 2. 調整座標軸範圍（axis）

- 用 `plt.axis([xmin, xmax, ymin, ymax])` 調整顯示範圍：

```python
plt.axis([-0.5, 10.5, -5, 105])
```

    - xmin、xmax：x 軸起訖值
    - ymin、ymax：y 軸起訖值

---

### 3. 圖例（legend）

- 在繪圖時加上 `label` 參數：

```python
plt.plot(x, y1, label="First")
plt.plot(x, y2, label="Second")
```

- 顯示圖例：

```python
plt.legend(loc="upper left")  # loc 可選 "upper right", "lower left" 等
```


---

### 4. 儲存圖檔（savefig）

- 存成 pdf、png 等格式：

```python
plt.savefig("myplot.pdf")  # 也可用 "myplot.png"
```

- 圖檔會儲存在目前工作目錄（working directory）。

---

### 5. 綜合範例

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(0, 10, 20)
y1 = x ** 2
y2 = x ** 1.5

plt.plot(x, y1, 'bo-', label="First", linewidth=2, markersize=4)
plt.plot(x, y2, 'gs-', label="Second", linewidth=2, markersize=4)

plt.xlabel("$x$")
plt.ylabel("$y$")
plt.axis([-0.5, 10.5, -5, 105])
plt.legend(loc="upper left")
plt.savefig("myplot.pdf")
plt.show()
```


---

### 6. 小提醒

- `xlabel`、`ylabel`、`legend`、`axis`、`savefig` 都可多次呼叫，最後一次設定會生效。
- 圖檔格式由檔名副檔名決定（如 `.pdf`, `.png`, `.jpg`）。
- 若在 Jupyter/IPython，`plt.show()` 可省略，圖會自動顯示。

---

**總結：**
這些元素讓你的圖表更專業、更易讀，也方便將結果保存成高品質圖片或 PDF，適合報告、論文或簡報使用。

