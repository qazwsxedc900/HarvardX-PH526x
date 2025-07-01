-----

## KNN 分類器：尋找 K 近鄰與預測類別

K 近鄰 (K-Nearest Neighbors, KNN) 算法的核心在於找到一個點的 K 個最近鄰居，並根據這些鄰居的類別進行多數投票來預測該點的類別。

### 1\. 尋找 K 近鄰 (`find_nearest_neighbors`)

**目標**：給定一個目標點 `p`，從數據集 `points` 中找出距離 `p` 最近的 `k` 個點的索引。

**偽程式碼**：

1.  遍歷所有點。
2.  計算點 `p` 與每個其他點之間的距離。
3.  對距離進行排序，並返回距離 `p` 最近的 `k` 個點。

**建立測試數據集**：
為了方便互動測試，我們首先建立一個簡單的二維 NumPy 陣列作為測試數據集 `points`，以及一個要分類的目標點 `p`。

```python
import numpy as np
import matplotlib.pyplot as plt

# 設定測試點集
points = np.array([
    [1, 1], [1, 2], [1, 3],
    [2, 1], [2, 2], [2, 3],
    [3, 1], [3, 2], [3, 3]
])

# 設定目標點 P
p = np.array([2.5, 2])

# 視覺化數據
plt.plot(points[:, 0], points[:, 1], 'ro', markersize=8, label='數據點') # 紅色圓圈表示所有數據點
plt.plot(p[0], p[1], 'bo', markersize=10, label='目標點 P') # 藍色圓圈表示點 P
plt.axis([0.5, 3.5, 0.5, 3.5]) # 設定軸的範圍，使點更清晰
plt.grid(True)
plt.title("數據點與目標點 P")
plt.xlabel("X 座標")
plt.ylabel("Y 座標")
plt.legend()
plt.show()
```

**計算距離並排序**：
我們需要計算 `p` 與 `points` 中每個點之間的距離。NumPy 的 `linalg.norm` 函數可以計算歐幾里德距離。`argsort` 函數則能返回排序後的索引。

```python
# 初始化一個空陣列來儲存距離
distances = np.zeros(points.shape[0])

# 遍歷所有點，計算距離
for i in range(len(distances)):
    distances[i] = np.linalg.norm(p - points[i])

# print("距離陣列:", distances)

# 使用 argsort 獲取排序後的索引
ind = np.argsort(distances)
# print("排序後的索引:", ind)

# 驗證距離是否按索引排序
# print("按距離排序後的距離:", distances[ind])

# 獲取最近的 K 個點的索引 (例如 K=2)
k = 2
nearest_k_indices = ind[:k]
# print(f"最近的 {k} 個點的索引:", nearest_k_indices)
# print(f"最近的 {k} 個點的座標:\n", points[nearest_k_indices])
```

**`find_nearest_neighbors` 函數定義**：

```python
def find_nearest_neighbors(p, points, k=5):
    """
    找到點 p 的 K 個最近鄰居並返回它們的索引。

    參數：
    p (numpy.ndarray): 要分類的目標點。
    points (numpy.ndarray): 現有的數據點（訓練數據）。
    k (int): 要查找的最近鄰居數量。默認為 5。

    返回：
    numpy.ndarray: K 個最近鄰居在 points 陣列中的索引。
    """
    distances = np.zeros(points.shape[0])
    for i in range(len(distances)):
        distances[i] = np.linalg.norm(p - points[i]) # 計算歐幾里德距離

    # 使用 argsort 獲取距離排序後的索引
    ind = np.argsort(distances)

    # 返回前 k 個最近鄰居的索引
    return ind[:k]

# 測試函數
print("\n--- 測試 find_nearest_neighbors 函數 ---")
nearest_2_indices = find_nearest_neighbors(p, points, k=2)
print(f"點 P ({p}) 最近的 2 個鄰居索引: {nearest_2_indices}")
print(f"對應的點座標:\n {points[nearest_2_indices]}")

nearest_3_indices = find_nearest_neighbors(p, points, k=3)
print(f"點 P ({p}) 最近的 3 個鄰居索引: {nearest_3_indices}")
print(f"對應的點座標:\n {points[nearest_3_indices]}")
```

### 2\. KNN 預測 (`knn_predict`)

**目標**：根據 K 個最近鄰居的多數投票來預測新點 `p` 的類別。

**偽程式碼**：

1.  找到 K 個最近鄰居。
2.  根據這些鄰居的類別進行多數投票來預測 `p` 的類別。

**`knn_predict` 函數定義**：
這個函數將利用之前定義的 `find_nearest_neighbors` 函數和 `majority_vote` 函數。

```python
# 假設 majority_vote 函數已經定義 (參見之前的章節)
# from your_module import majority_vote # 或者直接複製過來

# 假設這是之前定義的 majority_vote 函數
import random
def majority_vote(votes):
    vote_counts = {}
    for vote in votes:
        if vote in vote_counts:
            vote_counts[vote] += 1
        else:
            vote_counts[vote] = 1
    max_count = max(vote_counts.values())
    winners = []
    for vote, count in vote_counts.items():
        if count == max_count:
            winners.append(vote)
    return random.choice(winners)


def knn_predict(p, points, outcomes, k=5):
    """
    使用 K 近鄰算法預測新點 p 的類別。

    參數：
    p (numpy.ndarray): 要分類的目標點。
    points (numpy.ndarray): 現有的數據點（訓練數據）。
    outcomes (numpy.ndarray): 訓練數據點對應的類別標籤。
    k (int): 要查找的最近鄰居數量。默認為 5。

    返回：
    int 或 str: 點 p 的預測類別。
    """
    # 步驟 1: 找到 K 個最近鄰居的索引
    ind = find_nearest_neighbors(p, points, k)

    # 步驟 2: 獲取這些鄰居的類別
    # outcomes[ind] 將從 outcomes 陣列中提取對應 ind 索引的類別
    nearest_outcomes = outcomes[ind]

    # 步驟 3: 進行多數投票來預測類別
    prediction = majority_vote(nearest_outcomes)
    return prediction

```

### 測試 `knn_predict` 函數

我們需要為我們的 `points` 數據集添加對應的 `outcomes`（類別標籤）。

```python
# 為我們的測試點定義類別 (0 或 1)
outcomes = np.array([0, 0, 0, 0, 1, 1, 1, 1, 1]) # 前 4 個點為類別 0，後 5 個點為類別 1
print("\n--- 測試 knn_predict 函數 ---")
print(f"數據點的類別：{outcomes}")

# 測試點 P1: (2.5, 2.7)
p1 = np.array([2.5, 2.7])
k_val = 2
predicted_class_p1 = knn_predict(p1, points, outcomes, k=k_val)
print(f"\n點 P1 ({p1}) 的預測類別 (K={k_val}): {predicted_class_p1}")
# 根據視覺化，(2.5, 2.7) 應該會更接近類別 1 的點

# 測試點 P2: (1.0, 2.7)
p2 = np.array([1.0, 2.7])
k_val = 2
predicted_class_p2 = knn_predict(p2, points, outcomes, k=k_val)
print(f"點 P2 ({p2}) 的預測類別 (K={k_val}): {predicted_class_p2}")
# 根據視覺化，(1.0, 2.7) 應該會更接近類別 0 的點
```

根據此程式碼，我們可以嘗試多個不同的點，並觀察它們被歸類為 0 或 1。接下來，我們將生成更有趣的合成數據來進一步探索。

-----
