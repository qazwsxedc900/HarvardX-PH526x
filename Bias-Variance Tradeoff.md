-----

## 繪製 KNN 分類器的預測網格與 K 值選擇

為了視覺化 KNN 分類器如何劃分預測空間，我們需要一個函數來繪製預測網格。這個函數通常稱為 `plot_prediction_grid`，它會將 `make_prediction_grid` 函數生成的結果繪製出來。

### `plot_prediction_grid` 函數

```python
import numpy as np
import matplotlib.pyplot as plt

# 確保之前的函數已定義或導入 (例如來自 generate_synthetic_data, make_prediction_grid 的定義)
# 這裡假設這些函數已經在運行環境中
# from your_module import generate_synthetic_data, make_prediction_grid

# 再次提供必要的函數定義，以確保代碼可獨立運行
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

def generate_synthetic_data(n=50):
    mean0 = np.array([0, 0])
    cov0 = np.array([[1, 0], [0, 1]])
    mean1 = np.array([1, 1])
    cov1 = np.array([[1, 0], [0, 1]])
    points0 = np.random.multivariate_normal(mean0, cov0, size=(n, 2))
    points1 = np.random.multivariate_normal(mean1, cov1, size=(n, 2))
    points = np.concatenate((points0, points1), axis=0)
    outcomes = np.concatenate((np.repeat(0, n), np.repeat(1, n)), axis=0)
    return points, outcomes

def make_prediction_grid(predictors, outcomes, limits, h, k):
    x_min, x_max, y_min, y_max = limits
    xs = np.arange(x_min, x_max, h)
    ys = np.arange(y_min, y_max, h)
    xx, yy = np.meshgrid(xs, ys)
    prediction_grid = np.zeros(xx.shape, dtype=int)
    for i, x in enumerate(xs):
        for j, y in enumerate(ys):
            p = np.array([x, y])
            prediction = knn_predict(p, predictors, outcomes, k)
            prediction_grid[j, i] = prediction
    return xx, yy, prediction_grid


def plot_prediction_grid(xx, yy, prediction_grid, filename):
    """
    繪製預測網格的決策邊界。

    參數：
    xx (numpy.ndarray): 網格點的 X 座標。
    yy (numpy.ndarray): 網格點的 Y 座標。
    prediction_grid (numpy.ndarray): 網格上每個點的預測類別。
    filename (str): 保存圖表的文件名。
    """
    background_colormap = plt.cm.RdBu # 紅藍色圖
    plt.figure(figsize=(10, 10))
    plt.pcolormesh(xx, yy, prediction_grid, cmap=background_colormap, alpha=0.5)
    plt.xlabel("X")
    plt.ylabel("Y")
    plt.title(f"KNN Decision Boundary (K={filename.split('_')[-2].replace('k','')})") # 從文件名提取 K 值
    plt.colorbar(label="Predicted Class (0 or 1)")
    plt.tight_layout()
    plt.savefig(filename)
    plt.close() # 關閉圖形，防止在循環中重疊

```

### 實驗不同的 K 值

我們可以嘗試不同的 K 值來觀察決策邊界的變化。K 值越小，決策邊界越複雜；K 值越大，決策邊界越平滑。

```python
# 1. 生成合成數據
# 每個類別生成 50 個點，共 100 個點
predictors, outcomes = generate_synthetic_data(n=50)

print(f"預測變數形狀: {predictors.shape}")
print(f"結果類別形狀: {outcomes.shape}")

# 定義繪圖限制和步長
plot_limits = (-3, 4, -3, 4) # (x_min, x_max, y_min, y_max)
grid_step_size = 0.1

# 2. 實驗 K = 5
k_value_5 = 5
file_name_5 = f"knn_synth_k{k_value_5}.pdf" # 使用 f-string 方便包含 k 值

# 生成預測網格
xx_5, yy_5, prediction_grid_5 = make_prediction_grid(
    predictors, outcomes, plot_limits, grid_step_size, k_value_5
)

# 繪製預測網格
plot_prediction_grid(xx_5, yy_5, prediction_grid_5, file_name_5)
print(f"圖表已保存為: {file_name_5}")


# 3. 實驗 K = 50
k_value_50 = 50
file_name_50 = f"knn_synth_k{k_value_50}.pdf"

# 生成預測網格
xx_50, yy_50, prediction_grid_50 = make_prediction_grid(
    predictors, outcomes, plot_limits, grid_step_size, k_value_50
)

# 繪製預測網格
plot_prediction_grid(xx_50, yy_50, prediction_grid_50, file_name_50)
print(f"圖表已保存為: {file_name_50}")

print("\n請檢查生成的 PDF 文件 'knn_synth_k5.pdf' 和 'knn_synth_k50.pdf'，比較決策邊界平滑度。")
```

### 結果分析與 K 值選擇

當您比較 `knn_synth_k5.pdf` 和 `knn_synth_k50.pdf` 時，您會觀察到：

  * **K = 5 (較小的 K 值)**：決策邊界會更加**複雜和鋸齒狀**，它會更緊密地跟隨訓練數據點的局部模式。這表示模型對訓練數據的噪聲（noise）更敏感，可能會導致**過度擬合 (overfitting)**。

  * **K = 50 (較大的 K 值)**：決策邊界會更加**平滑**，因為它考慮了更多鄰居的投票，從而平均了局部變化。這使得模型對訓練數據的噪聲不那麼敏感，但如果 K 值過大，可能會導致**欠擬合 (underfitting)**，即模型無法捕捉數據的真實模式。

這個現象就是所謂的**偏差-變異度權衡 (Bias-Variance Tradeoff)**。

  * **偏差 (Bias)**：模型對數據的假設過於簡化，導致無法捕捉數據中的複雜關係（通常發生在 K 值過大時，模型過於平滑）。
  * **變異度 (Variance)**：模型對訓練數據中的隨機性噪聲過於敏感，導致在不同訓練數據集上表現差異大（通常發生在 K 值過小時，模型過於複雜）。

尋找一個能夠在偏差和變異度之間取得平衡的 K 值是 KNN 算法中一個重要的超參數調優問題。在實際應用中，通常會通過交叉驗證 (Cross-validation) 等技術來選擇最佳的 K 值。在這個範例中，K=5 是一個合理的選擇，因為它在捕捉數據模式的同時，也避免了過度平滑。

-----
