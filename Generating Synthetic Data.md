-----

## 生成合成數據：二元常態分佈

為了測試我們的 K 近鄰 (KNN) 分類器，我們需要一些數據。這裡我們將編寫一個函數，生成兩組數據點：一組來自類別 0，另一組來自類別 1。這類由電腦生成的數據被稱為**合成數據 (synthetic data)**。

我們將從兩個**二元常態分佈 (bivariate normal distributions)** 生成預測變數（即 x 和 y 座標）。第一個分佈產生屬於類別 0 的觀測值，第二個分佈產生屬於類別 1 的觀測值。

「二元 (bivariate)」表示有兩個變數（如 x 和 y）。如果只生成一個變數（如僅 x），則為單元數據。由於數據是我們自己生成的，我們知道預期的結果，這在開發程式碼時非常有用。

### 導入與數據生成

我們將使用 `numpy` 的 `random.multivariate_normal` 函數來生成數據，它來自 `numpy.random` 模組，但文件中提到的 `ipstats` 模組可能是另一個環境或庫的簡寫，通常在 Python 中，我們會直接使用 `numpy` 或 `scipy.stats` 來處理常態分佈。這裡我們假定使用 `numpy.random.multivariate_normal`。

```python
import numpy as np
import matplotlib.pyplot as plt

# 導入之前的函數，確保代碼完整性
# 假設您已經定義了 find_nearest_neighbors, majority_vote, knn_predict

# 範例：生成來自二元常態分佈的點
# 指定類別 0 的均值和標準差 (或協方差矩陣)
mean0 = np.array([0, 0])
cov0 = np.array([[1, 0], [0, 1]]) # 單位協方差矩陣，表示 x 和 y 不相關，且方差為 1

# 生成來自 class 0 的 5 個觀測值 (每行 2 列)
# np.random.multivariate_normal(mean, cov, size)
# size=(rows, cols) 表示生成多少個 2 維的點
data_class0 = np.random.multivariate_normal(mean0, cov0, size=(5, 2))
# print("Class 0 數據:\n", data_class0)

# 指定類別 1 的均值和標準差
mean1 = np.array([1, 1]) # 均值移到 (1,1)
cov1 = np.array([[1, 0], [0, 1]]) # 假設與 class 0 相同的協方差

# 生成來自 class 1 的 5 個觀測值
data_class1 = np.random.multivariate_normal(mean1, cov1, size=(5, 2))
# print("\nClass 1 數據:\n", data_class1)

# 連接兩個陣列
# 沿著軸 0 (行) 進行連接
points = np.concatenate((data_class0, data_class1), axis=0)
# print("\n連接後的 points 陣列 (10 行 2 列):\n", points)
# print("形狀:", points.shape)
```

### 生成合成數據函數 `generate_synthetic_data`

現在，我們將上述邏輯封裝到一個函數中，允許我們指定每個類別的觀測值數量 `n`。

```python
def generate_synthetic_data(n=50):
    """
    生成兩組數據點，分別來自兩個不同的二元常態分佈。
    第一組數據點屬於類別 0，第二組屬於類別 1。

    參數：
    n (int): 每個類別的觀測點數量。默認為 50。

    返回：
    tuple: (points, outcomes)
           - points (numpy.ndarray): 包含所有數據點的陣列，形狀為 (2*n, 2)。
           - outcomes (numpy.ndarray): 包含每個點對應類別標籤的陣列，形狀為 (2*n,)。
    """
    # 定義兩個類別的均值和協方差
    mean0 = np.array([0, 0])
    cov0 = np.array([[1, 0], [0, 1]])

    mean1 = np.array([1, 1])
    cov1 = np.array([[1, 0], [0, 1]])

    # 生成來自類別 0 的 n 個觀測值
    points0 = np.random.multivariate_normal(mean0, cov0, size=(n, 2))
    # 生成來自類別 1 的 n 個觀測值
    points1 = np.random.multivariate_normal(mean1, cov1, size=(n, 2))

    # 連接所有數據點
    points = np.concatenate((points0, points1), axis=0)

    # 生成對應的類別標籤 (outcome)
    # n 個 0，後面跟著 n 個 1
    outcomes = np.concatenate((np.repeat(0, n), np.repeat(1, n)), axis=0)

    return points, outcomes

```

### 測試與視覺化合成數據

```python
# 為了範例，我們設定 n = 20
n = 20
points, outcomes = generate_synthetic_data(n)

print(f"生成的數據點形狀: {points.shape}")
print(f"生成的類別標籤形狀: {outcomes.shape}")

# 繪製生成的數據點
plt.figure(figsize=(8, 8))

# 繪製類別 0 的點 (前 n 行)，使用紅色圓圈
plt.plot(points[:n, 0], points[:n, 1], 'ro', label='Class 0')

# 繪製類別 1 的點 (後 n 行)，使用藍色圓圈
plt.plot(points[n:, 0], points[n:, 1], 'bo', label='Class 1')

plt.title("合成二元常態分佈數據")
plt.xlabel("X 座標")
plt.ylabel("Y 座標")
plt.grid(True)
plt.legend()
plt.savefig("bivariate_data.pdf") # 保存為 PDF 文件
plt.show()

```

通過運行這段代碼，我們將生成一個包含 2\*n 個數據點的散佈圖，其中 n 個紅色點代表類別 0，n 個藍色點代表類別 1。這些合成數據將是我們進一步測試 KNN 分類器的理想基礎。

-----
