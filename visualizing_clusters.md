# 視覺化威士忌風味群集筆記

## 目標

將我們剛剛透過「光譜共群」發現的威士忌群集，在原始的威士忌 DataFrame 中進行視覺化呈現。我們將重新排序 DataFrame，使得屬於同一個群集的威士忌排在一起，從而讓群集結構在視覺上一目了然。

---

## Python 實作步驟

### 1. 將群集標籤加入 DataFrame 並重新排序

我們可以用三行核心程式碼完成這個操作：

1.  **提取標籤並加入表格**：首先，從訓練好的模型 (`model`) 中提取出每一筆資料（威士忌）對應的群集標籤 (`row_labels_`)，然後將這些標籤作為新的一列（例如，名為 'Group'）附加到原始的威士忌 DataFrame (`whisky_df`) 中。

2.  **按群集標籤排序**：接著，我們根據剛剛加入的 'Group' 列，對整個 DataFrame 進行升序排序。這樣，來自同一個群集的威士忌就會被排在一起。

3.  **重設索引**：排序後，DataFrame 的原始索引會被打亂。我們呼叫 `reset_index()` 來重新產生一組從 0 開始的連續索引。

```python
# 1. 從模型中提取群集標籤，並將其作為新的一列 'Group' 加入 whisky DataFrame
whisky_df['Group'] = pd.Series(model.row_labels_, index=whisky_df.index)

# 2. 根據 'Group' 這一列的值，對整個 DataFrame 進行排序
whisky_df = whisky_df.sort_values(by='Group')

# 3. 重設索引，drop=True 表示不保留舊的索引
whisky_df = whisky_df.reset_index(drop=True)
```

### 2. 重新計算相關係數矩陣

因為我們已經重新排列了 DataFrame 的行（代表不同的威士忌），所以我們需要基於這個新的順序，重新計算風味特徵之間的相關係數矩陣。

```python
import pandas as pd
import numpy as np

# 使用 .iloc 選取所有行，以及第 2 到 13 欄（假設這些是風味特徵欄位）
flavors = whisky_df.iloc[:, 2:14]

# 計算新的相關係數矩陣
correlations = flavors.corr()

# 將 pandas DataFrame 格式的相關係數矩陣轉換為 NumPy 陣列
# 這是因為後續繪圖函式可能更適合處理 NumPy 陣列
correlations_array = np.array(correlations)
```

> **說明**：`pd.DataFrame.corr()` 返回的是一個 Pandas DataFrame。為了方便後續的數學運算或繪圖，我們經常會將其轉換為 NumPy 陣列。

### 3. 繪製比較圖

現在，我們將原始的相關係數矩陣和重新排序後的相關係數矩陣並排繪製出來，以便進行比較。

我們可以使用 `matplotlib` 的 `pcolor` 或 `imshow` 函式來繪製熱力圖 (heatmap)。

```python
import matplotlib.pyplot as plt

# 假設 core_whisky 是原始的相關係數矩陣
# correlations_array 是我們剛計算出的、重新排序後的矩陣

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(10, 5))

# 繪製左圖：原始相關係數矩陣
ax1.set_title('Original Correlation Matrix')
ax1.pcolor(core_whisky)

# 繪製右圖：重排後的相關係數矩陣
ax2.set_title('Reordered Correlation Matrix')
ax2.pcolor(correlations_array)

plt.show()
# 或者儲存圖片
# plt.savefig('whisky_correlation_comparison.png')
```

### 4. 解讀結果

執行繪圖程式碼後，我們會得到一張包含兩張熱力圖的圖片：

- **左圖 (原始矩陣)**：這是在未經排序的威士忌資料上計算的相關係數矩陣。顏色分佈看起來可能比較雜亂，看不出明顯的結構。

- **右圖 (重排後矩陣)**：這是我們根據光譜共群結果重新排序後得到的矩陣。在這張圖中，你會清楚地看到沿著對角線（從左下到右上）形成了 **六個** 明顯的 **色塊 (blocks)**。

**結論：**

這些色塊就是我們找到的六個威士忌群集。這意味著，**屬於同一個色塊內的威士忌，在它們的風味特徵（如煙燻味、蜂蜜味等）上彼此更為相似**。

---

這項案例研究的影片部分到此結束。我們為您準備了額外的練習，以便您繼續探索這個案例。希望您喜歡這次關於蘇格蘭威士忌的案例研究。
