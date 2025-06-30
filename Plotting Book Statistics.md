-----

## Pandas 表格操作與數據視覺化

在處理數據時，能夠從 `pandas` 表格中輕鬆提取特定欄位並進行視覺化是核心技能。

### 提取特定欄位

我們可以通過欄位名稱直接存取 `pandas` `DataFrame` 中的特定列。例如：

```python
# 假設 stats 是我們之前建立的 DataFrame
# stats.length 會返回一個 Series，包含所有書籍的總詞數
print(stats.length)

# stats.unique 會返回一個 Series，包含所有書籍的獨特詞數
print(stats.unique)
```

這樣，我們就能方便地獲取所需數據進行後續分析。

-----

### 使用 Matplotlib 進行基本繪圖

為了進行數據繪圖，我們需要導入 `matplotlib.pyplot` 模組，通常會將其縮寫為 `plt`。

```python
import matplotlib.pyplot as plt

# 嘗試繪製一個簡單的散佈圖
# x 軸為書籍長度 (length)，y 軸為獨特詞數 (unique)
# 使用藍色圓圈作為標記 ('bo')
plt.plot(stats.length, stats.unique, 'bo')

# 顯示圖表
plt.show()
```

在這個散佈圖中，y 軸代表獨特單詞的數量，x 軸代表書籍的長度（以單詞數量計）。

### 對數座標圖 (Log-Log Plot)

有時候，將兩個軸都繪製成對數刻度可以更好地揭示數據的潛在關係。`matplotlib` 提供了 `loglog` 函數來實現這一點。

```python
# 繪製對數-對數散佈圖
plt.loglog(stats.length, stats.unique, 'bo')

# 顯示圖表
plt.show()
```

在對數-對數圖上，如果數據點呈現一條直線，這通常表明變數之間存在冪律關係，這將為後續的數據建模提供思路。

-----

### Pandas 數據篩選與多語言圖表繪製

`pandas` 允許我們根據特定條件輕鬆篩選數據，例如按語言進行篩選。

```python
# 篩選出語言為英文的數據
english_data = stats[stats.language == 'English']
print("英文書籍數據：")
print(english_data)

# 篩選出語言為法文的數據
french_data = stats[stats.language == 'French']
print("\n法文書籍數據：")
print(french_data)
```

我們可以利用這種數據篩選功能，為不同語言的書籍繪製不同顏色的數據點，使圖表更具洞察力。

#### 繪製多語言對數-對數圖

以下程式碼將展示如何為英文、法文、德文和葡萄牙文書籍使用不同的顏色繪製對數-對數散佈圖，並添加圖例和軸標籤。

```python
# 設置圖表大小
plt.figure(figsize=(10, 10))

# 繪製英文數據，使用深紅色圓圈標記
subset = stats[stats.language == 'English']
plt.loglog(subset.length, subset.unique, 'o', label='English', color='crimson')

# 繪製法文數據，使用森林綠圓圈標記
subset = stats[stats.language == 'French']
plt.loglog(subset.length, subset.unique, 'o', label='French', color='forestgreen')

# 繪製德文數據，使用橙色圓圈標記
subset = stats[stats.language == 'German']
plt.loglog(subset.length, subset.unique, 'o', label='German', color='orange')

# 繪製葡萄牙文數據，使用藍紫色圓圈標記
subset = stats[stats.language == 'Portuguese']
plt.loglog(subset.length, subset.unique, 'o', label='Portuguese', color='blueviolet')

# 添加圖例
plt.legend()

# 添加軸標籤
plt.xlabel("書籍長度 (單詞數量)")
plt.ylabel("獨特單詞數量")

# 保存圖表為 PDF 文件
plt.savefig("lang_plot.pdf")

# 顯示圖表 (在實際執行中，如果保存了文件，可能不需要顯示)
plt.show()
```

#### 圖表分析

生成的圖表（`lang_plot.pdf`）將在 x 軸上顯示書籍長度（從約 1,000 到 100 萬），在 y 軸上顯示獨特單詞數量（從約 1,000 到 10 萬），兩個軸都採用對數刻度。不同顏色的點代表不同語言的書籍。

這張圖清晰地展示了書籍長度與獨特單詞數量之間的關係，並且可以視覺化不同語言之間的差異。

-----

### 總結

通過這個案例研究，我們學會了多項實用技能：

  * 計算單詞頻率。
  * 導航文件目錄並遍歷多層子目錄。
  * 使用 `collections` 模組中的 `Counter` 物件進行高效計數。
  * 掌握了 `pandas` 的基本概念，包括創建 `DataFrame`、添加數據和提取特定欄位。
  * 利用 `matplotlib` 進行數據視覺化，包括創建散佈圖和對數-對數圖。

希望您喜歡這個案例研究，並準備好嘗試更多練習！
