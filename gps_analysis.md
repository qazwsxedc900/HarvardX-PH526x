# GPS 鳥類追蹤數據分析學習筆記

## 導論

全球定位系統（GPS）提供地球上任何地方的位置資訊。它由美國政府建立和維護，任何擁有 GPS 接收器的人都可以免費使用。GPS 用途廣泛，從商業到軍事應用都有。現在，GPS 接收器已整合到大多數智慧型手機中，使其普及化。

科學家們對 GPS 有許多專門的用途。一個引人入勝的研究領域是利用 GPS 追蹤動物的活動。現在可以製造小型的太陽能充電 GPS 設備，無需更換電池，可用於追蹤鳥類的飛行模式。

## 案例研究：鳥類追蹤

本案例將介紹如何操作、檢查和視覺化用於追蹤鳥類的 GPS 數據。

### 數據來源

本案例的數據來自 LifeWatch INBO 計畫。該計畫發布了數個數據集。我們將使用一個小型數據集，其中包含名為 Eric、Nico 和 Sanne 的三隻海鷗的遷徙數據。

### 數據格式

CSV 檔案包含八個欄位，包括緯度、經度、海拔和時間戳等變數。

### 學習目標

在本案例中，我們將：

1.  載入數據
2.  視覺化簡單的飛行軌跡
3.  追蹤飛行速度
4.  了解日間飛行等更多內容

## 數據載入與初步分析

我們將使用 `pandas` 來讀取和分析 `bird_tracking.csv` 這個檔案。

### 1. 匯入 Pandas

首先，我們匯入 `pandas` 函式庫。

```python
import pandas as pd
```

### 2. 讀取 CSV 檔案

我們將數據讀取到一個名為 `birddata` 的 DataFrame 中。

```python
birddata = pd.read_csv("bird_tracking.csv")
```

### 3. 查看數據基本資訊

使用 `.info()` 方法可以取得數據集的簡要摘要。

```python
birddata.info()
```

從輸出中，我們可以得知此數據集包含約 62,000 個條目以及各欄位的資訊。

### 4. 查看數據前幾行

使用 `.head()` 方法可以查看數據集的前五行，有助於我們快速了解數據的結構。

```python
birddata.head()
```

花點時間仔細研究這個 DataFrame 的細節，特別是如果您是第一次接觸這類數據。

## 飛行軌跡視覺化

### 單一鳥類軌跡繪製

我們將從繪製單一鳥類的飛行軌跡開始。需要注意的是，直接將經緯度繪製在二維平面上會產生嚴重的失真，因為經緯度是球面座標。這對於大範圍的軌跡尤其明顯。不過，這是一個快速檢視數據並建立直觀感受的方法。後續我們會介紹更精確的地圖投影方法。

#### 1. 匯入函式庫

```python
import matplotlib.pyplot as plt
import numpy as np
```

#### 2. 篩選單一鳥類數據 (Eric)

```python
ix = birddata.bird_name == "Eric"
```

#### 3. 提取經緯度座標

```python
x, y = birddata.longitude[ix], birddata.latitude[ix]
```

#### 4. 繪製軌跡圖

```python
plt.figure(figsize=(7, 7))
plt.plot(x, y, "b.")
plt.xlabel("Longitude")
plt.ylabel("Latitude")
plt.title("Eric's Flight Trajectory")
plt.savefig("eric_trajectory.pdf") # 可選：儲存圖片
plt.show()
```

從圖中，我們可以看到 Eric 的遷徙路徑。儘管有失真，但我們仍能對其飛行路徑有一個大致的了解。

### 所有鳥類軌跡繪製

接下來，我們將在同一張圖上繪製所有三隻鳥的軌跡，以便進行比較。

#### 1. 取得所有鳥類名稱

```python
bird_names = pd.unique(birddata.bird_name)
```

#### 2. 迴圈繪製所有鳥類軌跡

```python
plt.figure(figsize=(7, 7))

for bird_name in bird_names:
    ix = birddata.bird_name == bird_name
    x, y = birddata.longitude[ix], birddata.latitude[ix]
    plt.plot(x, y, ".", label=bird_name)

plt.xlabel("Longitude")
plt.ylabel("Latitude")
plt.legend(loc="lower right")
plt.title("Flight Trajectories of All Birds")
plt.savefig("all_trajectories.pdf") # 可選：儲存圖片
plt.show()
```

從這張圖中，我們可以看到三隻鳥的整體飛行模式非常相似。然而，Nico 和 Sanne 的飛行範圍似乎比 Eric 更偏南。我們將在後續的分析中更詳細地研究這些軌跡。

## 飛行速度分析

數據中還包含鳥類二維速度的估計值，即牠們在地球曲面局部近似的二維平面上的飛行速度。讓我們來詳細研究一下這些速度數據。

### 處理缺失值 (NaN)

當我們嘗試直接繪製速度的直方圖時，可能會遇到錯誤。這是因為數據中包含了非數值（NaN）的條目。

#### 1. 檢查並計算 NaN

我們需要先檢查並處理這些缺失值。

```python
# 篩選出 Eric 的速度數據
ix = birddata.bird_name == "Eric"
speed = birddata.speed_2d[ix]

# 檢查是否存在 NaN
np.isnan(speed).any() # 返回 True

# 計算 NaN 的數量
np.sum(np.isnan(speed)) # 返回 85
```

#### 2. 移除 NaN 並繪圖

在繪圖前，我們必須過濾掉這些 NaN 值。

```python
# 找到所有非 NaN 的索引
ind = ~np.isnan(speed)

# 繪製過濾後的數據直方圖
plt.hist(speed[ind])
plt.show()
```

### 使用 Matplotlib 繪製美化的直方圖

現在我們有了可用的代碼，可以對其進行調整以獲得更美觀、資訊更豐富的圖表。

```python
plt.figure(figsize=(8, 4))
plt.hist(speed[ind], bins=np.linspace(0, 30, 20), density=True)
plt.xlabel("2D Speed (m/s)")
plt.ylabel("Frequency")
plt.title("Histogram of Eric's Flight Speed")
plt.savefig("hist_speed_eric.pdf")
plt.show()
```

在這個版本中，我們：
*   使用 `xlabel` 和 `ylabel` 添加了軸標籤。
*   使用 `bins` 和 `np.linspace` 指定了區間的範圍和數量。
*   將 `density` 設為 `True`，使直方圖的面積積分為 1，成為一個機率密度圖。

### 使用 Pandas 繪圖函式

Pandas 本身也提供了繪圖功能，雖然自訂性不如 Matplotlib，但它可以方便地處理像 NaN 這樣的問題。

```python
birddata.speed_2d.plot(kind='hist', range=[0, 30])
plt.xlabel("2D Speed (m/s)")
plt.ylabel("Frequency")
plt.title("Histogram of Flight Speeds")
plt.savefig("pd_hist.pdf")
plt.show()
```

使用 Pandas 的好處是我們不需要手動處理 NaN 值，Pandas 會在底層自動完成。這使得代碼更簡潔。不過，在繪圖前，養成檢查數據中是否存在 NaN 的習慣總是一個好主意。

## 時間戳處理與分析

在處理帶有時間戳的數據時，例如本案例中的 GPS 數據，我們經常需要對日期和時間戳進行算術運算，例如計算兩個觀測點之間的時間間隔。Python 的 `datetime` 模組就是為處理這類數據而設計的。

### 字串轉換為 Datetime 物件

我們的 `birddata` 中的 `date_time` 欄位目前是字串格式。為了進行時間運算，我們需要先將它們轉換為 `datetime` 物件。

#### 1. 匯入 `datetime` 模組

```python
import datetime
```

#### 2. 轉換單一時間戳

我們可以使用 `datetime.datetime.strptime()` 函式來進行轉換。這個函式需要兩個參數：要轉換的字串和指定其格式的字串。

```python
date_str = birddata.date_time[0]
# 移除 UTC 時區資訊 (+00)
date_time_obj = datetime.datetime.strptime(date_str[:-3], '%Y-%m-%d %H:%M:%S')
```

格式字串中的 `%Y`, `%m`, `%d`, `%H`, `%M`, `%S` 分別代表四位數的年份、月份、日期、小時、分鐘和秒。

#### 3. 轉換整個欄位並添加到 DataFrame

我們可以遍歷整個 `date_time` 欄位，將所有字串轉換為 `datetime` 物件，並將結果存儲在一個新的 `timestamp` 欄位中。

```python
timestamps = []
for k in range(len(birddata)):
    timestamps.append(datetime.datetime.strptime(birddata.date_time.iloc[k][:-3], "%Y-%m-%d %H:%M:%S"))
birddata["timestamp"] = pd.Series(timestamps, index=birddata.index)
```

### 時間差計算 (`timedelta`)

轉換為 `datetime` 物件後，我們就可以輕鬆地計算時間差。兩個 `datetime` 物件相減會得到一個 `timedelta` 物件。

```python
# 計算第四筆和第三筆觀測資料的時間差
time_diff = birddata.timestamp[4] - birddata.timestamp[3]
# time_diff 將會是一個 timedelta 物件
```

### 計算相對經過時間

我們可以計算從數據收集開始以來所經過的時間。

```python
# 篩選出 Eric 的時間戳
times = birddata.timestamp[birddata.bird_name == "Eric"]

# 計算從第一次觀測開始所經過的時間
elapsed_time = [time - times.iloc[0] for time in times]

# 將經過的時間轉換為以天為單位
elapsed_days = elapsed_time[1000] / datetime.timedelta(days=1)

# 將經過的時間轉換為以小時為單位
elapsed_hours = elapsed_time[1000] / datetime.timedelta(hours=1)
```

### 視覺化經過時間

我們可以繪製觀測次數與經過時間的關係圖，以了解數據收集的頻率。

```python
plt.plot(np.array(elapsed_time) / datetime.timedelta(days=1))
plt.xlabel("Observation")
plt.ylabel("Elapsed time (days)")
plt.title("Time Elapsed During Collection")
plt.show()
```

圖中的曲線如果不是一條完美的直線，說明觀測之間的時間間隔並不總是相同的。曲線中的跳躍表示數據收集中存在較長的時間間斷。這種數據探索有助於我們更深入地理解數據集。

## 每日平均速度分析

我們的下一個目標是繪製一張圖，Y 軸是每日平均速度，X 軸是以天為單位的時間。由於觀測數據的時間戳是不均勻的，我們需要一個演算法來按天分組數據並計算每天的平均速度。

### 演算法思路

1.  初始化 `next_day = 1`，一個用於儲存當天觀測索引的空列表 `inds`，以及一個儲存每日平均速度的空列表 `daily_mean_speed`。
2.  遍歷以天為單位的 `elapsed_days` 列表。
3.  如果當前觀測的天數小於 `next_day`，則將其索引添加到 `inds` 列表中。
4.  如果當前觀測的天數達到了 `next_day`，則：
    a.  使用 `inds` 中的索引，計算對應 `speed_2d` 的平均值。
    b.  將計算出的平均速度附加到 `daily_mean_speed` 列表中。
    c.  將 `next_day` 增加 1。
    d.  重置 `inds` 為空列表。
5.  循環結束後，繪製 `daily_mean_speed`。

### 程式碼實現

```python
# 假設 elapsed_days 已經計算出來
daily_mean_speed = []
next_day = 1
inds = []
for i, t in enumerate(elapsed_days):
    if t < next_day:
        inds.append(i)
    else:
        # 計算前一天的平均速度
        daily_mean_speed.append(np.mean(data.speed_2d[inds]))
        # 為新的一天重置
        next_day += 1
        inds = [i] # 將當前索引加入新的一天

# 繪圖
plt.figure(figsize=(8, 6))
plt.plot(daily_mean_speed)
plt.xlabel("Day")
plt.ylabel("Mean Speed (m/s)")
plt.title("Eric's Daily Mean Speed")
plt.savefig("daily_mean_speed.pdf")
plt.show()
```

從圖中，我們可以看到 Eric 大約在第 90 天和第 230 天有兩個速度高峰，這對應著牠的遷徙期。透過這種分析，我們能夠準確地識別出 Eric 的遷徙時間。

## 使用 Cartopy 進行地圖視覺化

為了更直觀地了解鳥類的遷徙路徑，我們可以使用 `Cartopy` 這個強大的地圖繪製工具庫，將飛行軌跡疊加在真實的地圖上。

### 1. 安裝 Cartopy

Cartopy 可以透過 `conda` 輕鬆安裝。在您的終端機中執行以下命令：

```bash
conda install -c conda-forge cartopy
```

### 2. 繪製帶有地圖投影的軌跡

安裝完成後，我們就可以使用它來繪製帶有地圖背景的軌跡圖了。

```python
import cartopy.crs as ccrs
import cartopy.feature as cfeature

# 選擇一個投影
proj = ccrs.Mercator()

plt.figure(figsize=(10, 10))
ax = plt.axes(projection=proj)

# 設定地圖範圍 (透過反覆試驗找到)
ax.set_extent((-25.0, 20.0, 52.0, 10.0))

# 添加地圖特徵
ax.add_feature(cfeature.LAND)
ax.add_feature(cfeature.OCEAN)
ax.add_feature(cfeature.COASTLINE)
ax.add_feature(cfeature.BORDERS, linestyle=':')

for bird_name in bird_names:
    ix = birddata["bird_name"] == bird_name
    x, y = birddata.longitude[ix], birddata.latitude[ix]
    # 使用 transform 參數進行座標轉換
    ax.plot(x, y, '.', transform=ccrs.Geodetic(), label=bird_name)

plt.legend(loc="upper left")
plt.savefig("map.pdf")
plt.show()
```

將飛行軌跡疊加在地圖上，為我們提供了關於鳥類遷徙模式的更深刻見解。我們不僅知道牠們何時遷徙，還能清楚地看到牠們從哪裡來，到哪裡去。

這只是 GPS 數據視覺化的基礎。您可以繼續探索，進行更詳細的飛行路徑分析。祝您玩得愉快！
