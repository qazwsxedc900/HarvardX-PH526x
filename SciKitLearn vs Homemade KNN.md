-----

## SciKitLearn 與自製 KNN 分類器：鳶尾花數據集應用

現在我們將把目光投向一個廣受歡迎的開源機器學習函式庫：**SciKitLearn (sklearn)**。儘管它功能非常廣泛，我們在這裡將重點使用其 K 近鄰分類器，並將其與我們自製的 KNN 分類器應用於經典的**鳶尾花 (Iris)** 數據集。

鳶尾花數據集由 Ron Fisher 於 1933 年創建，包含 150 種不同的鳶尾花，每種來自三個不同品種（每種 50 個）。對於每朵花，我們有四個協變數（或稱特徵）：花萼長度、花萼寬度、花瓣長度和花瓣寬度。

### 1\. 載入與探索鳶尾花數據集

首先，我們從 SciKitLearn 導入數據集並載入鳶尾花數據。

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn import datasets # 從 SciKitLearn 導入數據集
from sklearn.neighbors import KNeighborsClassifier # 導入 KNN 分類器

# 確保之前的函數已定義或導入 (例如 generate_synthetic_data, make_prediction_grid, plot_prediction_grid 等)
# 這裡假設這些函數已經在運行環境中

# 載入鳶尾花數據集
iris = datasets.load_iris()

# 查看數據的形狀
# iris.data 包含了特徵 (features)
# iris.target 包含了目標變數 (target variable, 即物種標籤)
print(f"鳶尾花數據形狀 (特徵): {iris.data.shape}") # 應該是 (150, 4)
print(f"鳶尾花目標形狀 (物種): {iris.target.shape}") # 應該是 (150,)

# 為了簡化，我們只使用前兩個特徵作為預測變數 (花萼長度, 花萼寬度)
predictors = iris.data[:, 0:2] # 所有行，第 0 和第 1 列
outcomes = iris.target # 鳶尾花的物種標籤 (0, 1, 2)

print(f"選取前兩個特徵後的預測變數形狀: {predictors.shape}")
print(f"結果類別標籤形狀: {outcomes.shape}")

```

### 2\. 視覺化鳶尾花數據

我們可以將數據點繪製出來，並用不同的顏色區分三個不同的物種。

```python
# 繪製鳶尾花數據點
plt.figure(figsize=(10, 8))

# 類別 0 (物種 Iris-setosa)
plt.plot(predictors[outcomes == 0, 0], predictors[outcomes == 0, 1], 'ro', label='Species 0 (Setosa)') # 紅色圓圈

# 類別 1 (物種 Iris-versicolor)
plt.plot(predictors[outcomes == 1, 0], predictors[outcomes == 1, 1], 'go', label='Species 1 (Versicolor)') # 綠色圓圈

# 類別 2 (物種 Iris-virginica)
plt.plot(predictors[outcomes == 2, 0], predictors[outcomes == 2, 1], 'bo', label='Species 2 (Virginica)') # 藍色圓圈

plt.xlabel(iris.feature_names[0]) # 花萼長度
plt.ylabel(iris.feature_names[1]) # 花萼寬度
plt.title("鳶尾花數據集 (前兩個特徵)")
plt.legend()
plt.grid(True)
plt.savefig("iris_data_plot.pdf")
plt.show()

```

在這張圖中，X 軸和 Y 軸分別對應花萼長度和花萼寬度。三個不同顏色的點代表了三個不同的鳶尾花物種。

### 3\. 繪製鳶尾花數據的預測網格

接下來，我們使用之前定義的 `make_prediction_grid` 和 `plot_prediction_grid` 函數，為鳶尾花數據集繪製預測網格。

```python
# 設定 K 值
k_iris = 5
# 定義輸出文件名
file_name_iris_grid = f"iris_grid_k{k_iris}.pdf"

# 設定網格限制 (根據鳶尾花數據範圍調整)
# 觀察數據點的範圍，X 軸大概在 4 到 8 之間，Y 軸大概在 1.5 到 4.5 之間
iris_limits = (4, 8, 1.5, 4.5)
grid_h = 0.1 # 步長

# 生成預測網格
xx_iris, yy_iris, prediction_grid_iris = make_prediction_grid(
    predictors, outcomes, iris_limits, grid_h, k_iris
)

# 繪製預測網格
plot_prediction_grid(xx_iris, yy_iris, prediction_grid_iris, file_name_iris_grid)
print(f"鳶尾花預測網格圖表已保存為: {file_name_iris_grid}")

```

在生成的鳶尾花預測網格圖中，您會看到三種不同顏色的區域，分別對應於分類器預測的三個鳶尾花物種。這張圖直觀地展示了 KNN 分類器的決策邊界：

  * 如果一個新數據點落在左上角的紅色區域，分類器會將其預測為紅色類別（物種 0）。
  * 如果落在左下角的藍色區域，則預測為藍色類別（物種 2）。
  * 如果落在右側的綠色區域，則預測為綠色類別（物種 1）。

### 4\. 使用 SciKitLearn 與自製 KNN 分類器

現在，我們將使用 SciKitLearn 庫中的 `KNeighborsClassifier` 和我們自製的 `knn_predict` 函數來進行分類，並比較它們的預測結果。

```python
# 使用 SciKitLearn 的 KNN 分類器
knn_sk = KNeighborsClassifier(n_neighbors=k_iris) # 設置鄰居數量為 5

# 擬合模型 (訓練模型)
knn_sk.fit(predictors, outcomes)

# 進行預測
sk_predictions = knn_sk.predict(predictors)

print(f"\nSciKitLearn 預測結果的形狀: {sk_predictions.shape}")
print(f"SciKitLearn 前 10 個預測: {sk_predictions[:10]}")


# 使用我們自製的 KNN 分類器進行預測
# 我們需要遍歷每個數據點並應用 knn_predict 函數
my_predictions = np.array([knn_predict(p, predictors, outcomes, k=k_iris) for p in predictors])

print(f"自製 KNN 預測結果的形狀: {my_predictions.shape}")
print(f"自製 KNN 前 10 個預測: {my_predictions[:10]}")

```

### 5\. 比較兩種分類器的預測結果

最後，我們來比較 SciKitLearn 的預測結果與我們自製分類器的預測結果，以及它們各自與實際類別標籤的一致性。

```python
# 比較 SciKitLearn 和自製 KNN 的預測一致性
agreement_sk_my = (sk_predictions == my_predictions).mean() * 100
print(f"\nSciKitLearn 和自製 KNN 預測的一致性: {agreement_sk_my:.2f}%")

# 比較 SciKitLearn 預測與實際結果的一致性 (準確度)
accuracy_sk = (sk_predictions == outcomes).mean() * 100
print(f"SciKitLearn 預測對實際結果的準確度: {accuracy_sk:.2f}%")

# 比較自製 KNN 預測與實際結果的一致性 (準確度)
accuracy_my = (my_predictions == outcomes).mean() * 100
print(f"自製 KNN 預測對實際結果的準確度: {accuracy_my:.2f}%")
```

**結果分析**：

  * 通常情況下，您會發現 SciKitLearn 的預測與我們自製 KNN 的預測結果**非常接近**，甚至可能完全一致（約 96% 的一致性）。這表明我們對 KNN 算法的實現是正確的。
  * 在對實際結果的準確度方面，SciKitLearn 在此特定訓練數據集上可能獲得約 83% 的準確度。有趣的是，我們的自製分類器在本例中可能會略勝一籌，達到約 85% 的準確度。這可能歸因於我們在 `majority_vote` 函數中處理平手時的**隨機選擇**策略，它有時會在特定數據集上帶來略微不同的結果。

**重要提示**：在訓練數據集上的準確度並不能完全代表模型在新數據上的表現。真正的模型性能應通過在獨立的測試集上進行評估來衡量，以避免對訓練數據的過度擬合。

### 結語

在這個案例研究中，我們深入探索了 KNN 分類器，從底層實現到使用專業庫 SciKitLearn。我們學習了：

  * 載入和處理真實世界的數據集。
  * 使用 KNN 算法進行分類。
  * 繪製預測網格以視覺化分類器的決策邊界。
  * 比較自製算法與成熟庫的實現，加深對算法細節的理解。

希望這些練習能幫助您更好地理解 KNN 分類和機器學習的基本概念。

-----
