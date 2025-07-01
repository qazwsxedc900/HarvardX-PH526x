-----

## K 近鄰分類器：多數投票函數

在構建 K 近鄰 (KNN) 分類器時，我們需要能夠計算「多數投票」。這意味著給定一系列的投票（例如數字 1、2、3），我們需要確定每個選項出現的次數，然後找出最常見的那個元素。舉例來說，如果投票結果是兩個 1、三個 2 和一個 3，那麼多數投票結果將是 2。

值得注意的是，雖然我們需要計算每個投票在序列中出現的次數，但我們不會返回計數本身，而是返回**計數最高**的那個選項。

### 構建 `majority_vote` 函數

我們將構建一個名為 `majority_vote` 的函數。

```python
import random # 導入 random 模組

def majority_vote(votes):
    """
    Given a sequence of votes, return the most common element.
    In case of a tie, a winner is picked uniformly at random.
    """
    vote_counts = {} # 將 word_counts 改為 vote_counts

    # 統計每個投票的次數
    for vote in votes: # 遍歷 votes 序列
        if vote in vote_counts:
            vote_counts[vote] += 1 # 增加計數器
        else:
            vote_counts[vote] = 1 # 初始化計數器為 1

    # 找出最大計數
    max_count = max(vote_counts.values())

    # 找出所有贏家 (可能有多個，如果平手)
    winners = []
    for vote, count in vote_counts.items(): # 遍歷字典的鍵值對
        if count == max_count:
            winners.append(vote) # 將贏家添加到列表中

    # 在所有贏家中隨機選取一個
    return random.choice(winners)
```

### 函數分解與說明

1.  **初始化計數字典**：
    我們將創建一個空字典 `vote_counts` 來儲存每個投票選項及其出現的次數。

2.  **統計投票**：
    我們遍歷 `votes` 序列中的每個 `vote`。

      * 如果 `vote` 已經在 `vote_counts` 中，則將其計數加一：`vote_counts[vote] += 1`。
      * 如果 `vote` 尚未出現，則在字典中創建新條目並將計數設置為 1：`vote_counts[vote] = 1`。

3.  **找出最大計數**：
    使用 `max(vote_counts.values())` 找出所有投票計數中的最大值，並將其儲存在 `max_count` 變數中。

4.  **找出所有贏家**：
    創建一個空列表 `winners`。我們遍歷 `vote_counts` 字典中的所有鍵值對 (使用 `.items()` 方法)。如果當前項目的計數 (`count`) 等於 `max_count`，則將該投票選項 (`vote`) 添加到 `winners` 列表中。這樣可以處理平手的情況。

5.  **隨機選取一個贏家**：
    最後，我們使用 `random.choice(winners)` 從 `winners` 列表中隨機選擇一個贏家並返回。即使有多個平手的情況，函數也只會返回一個單一的贏家。

### 測試 `majority_vote` 函數

```python
# 定義一些測試數據
votes_example_1 = [1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 3, 3, 3]
print(f"投票結果 1: {votes_example_1}")
winner_1 = majority_vote(votes_example_1)
print(f"多數投票結果 1: {winner_1}") # 應該是 3

# 添加更多 2 的投票來製造平手情況
votes_example_2 = [1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 3, 3, 3, 2, 2, 2, 2, 2, 2] # 3 和 2 都是 6 次
print(f"\n投票結果 2 (可能平手): {votes_example_2}")
# 運行多次，觀察結果在 2 和 3 之間隨機變化
print("多數投票結果 2 (多次運行):")
for _ in range(5): # 運行 5 次
    winner_2 = majority_vote(votes_example_2)
    print(f"  -> {winner_2}")
```

### 統計學中的「眾數」與 SciPy 捷徑

在統計學中，序列中最常出現的元素被稱為**眾數 (mode)**。查找眾數是一個常見的統計操作。

可以利用 SciPy 模組中的 `mode` 函數來快速實現這個功能。

```python
import numpy as np # mode 函數通常用於 NumPy 陣列
from scipy import stats as ss # 導入 scipy.stats 模組

def majority_vote_short(votes):
    """
    Return the most common element in votes using scipy.stats.mode.
    Note: In case of a tie, scipy.stats.mode returns the lowest value.
    """
    # ss.mode 返回一個 ModeResult 對象，包含眾數和對應的計數
    mode_result = ss.mode(votes)
    # 我們只需要眾數本身，即 result[0][0]
    return mode_result.mode[0]

# 測試 short 版本 (注意平手時的行為)
votes_example_2_np = np.array(votes_example_2) # 需要轉換為 NumPy 陣列
print(f"\n使用 short 版本 (SciPy) 進行投票結果 2: {votes_example_2_np}")
winner_short = majority_vote_short(votes_example_2_np)
print(f"SciPy 眾數結果: {winner_short}") # 在本例中，即使 2 和 3 平手，它通常會返回 2 (較小的值)

```

### 比較兩種實現方式

  * **`majority_vote` (自定義版本)**：

      * 優點：在平手時，可以實現**隨機選擇**一個贏家，這在某些 KNN 應用中是理想行為。
      * 缺點：需要手動實現計數和查找最大值的邏輯。

  * **`majority_vote_short` (使用 `scipy.stats.mode` )**：

      * 優點：程式碼非常簡潔高效，利用了成熟的科學計算庫。
      * 缺點：在平手時，`scipy.stats.mode` 函數通常會返回**值較小**的眾數（或其文檔中指定的其他規則），而不是隨機選擇。這可能不符合我們對 KNN 分類器中多數投票的特定需求。

## 因此，儘管使用 SciPy 的方式更短更快，但在需要平手時隨機選擇贏家的場景中，我們將繼續使用我們自定義的 `majority_vote` 函數。
