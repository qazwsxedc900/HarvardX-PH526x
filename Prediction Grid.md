-----

## 繪製預測網格 (Prediction Grid)

在數據分析中，尤其是在使用分類器之後，我們常常需要視覺化分類器在預測空間中的行為。**繪製預測網格**意味著我們將觀察預測變數空間的某個區域，並使用 KNN 分類器計算網格中每個點的類別預測。這樣，我們不僅能知道分類器如何分類給定點，還能了解它如何分類特定矩形區域內的所有點。

### `make_prediction_grid` 函數

我們將編寫一個名為 `make_prediction_grid` 的函數，它將接收多個參數。

```python
import numpy as np
import matplotlib.pyplot as plt

# 確保之前的函數已定義或導入
# from your_module import find_nearest_neighbors, majority_vote, knn_predict

# 假設這些函數已經被定義，這裡再次提供以確保代碼可運行
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

def find_nearest_neighbors(p, points, k=5):
    distances = np.zeros(points.shape[0])
    for i in range(len(distances)):
        distances[i] = np.linalg.norm(p - points[i])
    ind = np.argsort(distances)
    return ind[:k]

def knn_predict(p, points, outcomes, k=5):
    ind = find_nearest_neighbors(p, points, k)
    nearest_outcomes = outcomes[ind]
    prediction = majority_vote(nearest_outcomes)
    return prediction


def make_prediction_grid(predictors, outcomes, limits, h, k):
    """
    對預測網格上的每個點進行分類。

    參數：
    predictors (numpy.ndarray): 訓練數據的預測變數。
    outcomes (numpy.ndarray): 訓練數據的類別標籤。
    limits (tuple): (x_min, x_max, y_min, y_max)，定義網格的範圍。
    h (float): 網格的步長。
    k (int): KNN 算法中的 K 值。

    返回：
    tuple: (xx, yy, prediction_grid)
           - xx (numpy.ndarray): 網格點的 X 座標。
           - yy (numpy.ndarray): 網格點的 Y 座標。
           - prediction_grid (numpy.ndarray): 網格上每個點的預測類別。
    """
    # 解包網格限制
    x_min, x_max, y_min, y_max = limits

    # 生成 X 和 Y 軸上的值
    xs = np.arange(x_min, x_max, h)
    ys = np.arange(y_min, y_max, h)

    # 使用 NumPy 的 meshgrid 函數生成 2D 網格
    xx, yy = np.meshgrid(xs, ys)

    # 創建一個空的預測網格，大小與 xx 或 yy 相同，數據類型為整數
    prediction_grid = np.zeros(xx.shape, dtype=int)

    # 遍歷網格中的每個點，進行分類預測
    # 注意：這裡使用 enumerate 是為了同時獲取索引和值
    # prediction_grid[j, i] 的順序是 [row_index, col_index]，
    # 而 j 對應 y 值（行），i 對應 x 值（列）。
    for i, x in enumerate(xs):
        for j, y in enumerate(ys):
            p = np.array([x, y]) # 當前網格點的座標
            prediction = knn_predict(p, predictors, outcomes, k)
            prediction_grid[j, i] = prediction # 將預測結果儲存到網格中

    return xx, yy, prediction_grid

```

### `make_prediction_grid` 函數說明

1.  **參數**：

      * `predictors`：訓練數據的預測變數（點）。
      * `outcomes`：訓練數據的類別標籤。
      * `limits`：一個包含 `(x_min, x_max, y_min, y_max)` 的元組，定義了網格的範圍。
      * `h`：網格的步長，決定網格點的密度。
      * `k`：KNN 算法中使用的近鄰數量。

2.  **生成網格點**：

      * `np.arange(x_min, x_max, h)` 和 `np.arange(y_min, y_max, h)` 分別生成 X 和 Y 軸上的一維點序列。
      * `np.meshgrid(xs, ys)` 是一個關鍵函數，它將這兩個一維序列轉換為兩個二維陣列 `xx` 和 `yy`。`xx` 包含了網格中每個點的 X 座標，`yy` 包含了網格中每個點的 Y 座標。

3.  **預測網格初始化**：
    `prediction_grid = np.zeros(xx.shape, dtype=int)` 創建一個與 `xx` 或 `yy` 形狀相同的零陣列，用於儲存每個網格點的預測類別（0 或 1）。

4.  **遍歷網格並預測**：
    使用嵌套的 `for` 迴圈和 `enumerate` 遍歷 `xs` 和 `ys`。對於每個網格點 `(x, y)`：

      * 創建一個 NumPy 陣列 `p` 作為當前網格點。
      * 呼叫 `knn_predict(p, predictors, outcomes, k)` 來獲取該點的預測類別。
      * 將預測結果儲存到 `prediction_grid[j, i]` 中。**注意索引順序：`j` 對應行（Y 座標），`i` 對應列（X 座標）**。這是因為 `prediction_grid` 的第一個索引是行索引，通常代表 Y 軸，第二個索引是列索引，通常代表 X 軸。

5.  **返回值**：
    函數返回 `xx`、`yy` 和 `prediction_grid`。

### `meshgrid` 詳解

`meshgrid` 函數接受兩個或更多個座標向量，例如一個包含 X 軸感興趣值的向量和另一個包含 Y 軸感興趣值的向量。它返回多個矩陣（與輸入向量數量相同），第一個包含每個網格點的 X 值，第二個包含每個網格點的 Y 值，依此類推。

**範例**：
假設 `xs = [0, 1, 2]` 和 `ys = [10, 20]`。
`xx, yy = np.meshgrid(xs, ys)` 將會生成：

`xx`:

```
[[0, 1, 2],
 [0, 1, 2]]
```

`yy`:

```
[[10, 10, 10],
 [20, 20, 20]]
```

這兩個矩陣共同定義了 `(0,10), (1,10), (2,10), (0,20), (1,20), (2,20)` 這些網格點。

### `enumerate` 詳解

`enumerate` 是一個非常有用的內建函數，當我們在處理序列並同時需要訪問序列中的**元素**及其**索引**時，它能派上用場。

**範例**：

```python
seasons = ['spring', 'summer', 'fall', 'winter']

# 傳統遍歷 (只獲取元素)
print("傳統遍歷:")
for season in seasons:
    print(season)

# 使用 enumerate
print("\n使用 enumerate:")
for index, season in enumerate(seasons):
    print(f"索引: {index}, 季節: {season}")

# enumerate 返回一個 enumerate 對象，可以轉換為列表來查看其內容
enum_obj = enumerate(seasons)
print("\nenumerate 對象轉換為列表:")
print(list(enum_obj))
# 輸出: [(0, 'spring'), (1, 'summer'), (2, 'fall'), (3, 'winter')]
```

`enumerate` 返回一個由元組組成的序列，每個元組包含 `(索引, 元素)`。這使得在迴圈中同時使用索引和值變得非常方便和 Pythonic。

-----
