<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## Python 迴圈機制解析（For Loop 與 While Loop）

### 一、For Loop 基本運作原理

- **序列迭代機制**：
將序列中的元素逐個賦值給目標變數，並執行對應程式區塊。

```python
for target in sequence:
    # 執行區塊
```

- **流程圖解**：

```
序列 [元素0, 元素1, 元素2]
↓ 迭代過程
第1次迴圈：target → 元素0 → 執行區塊
第2次迴圈：target → 元素1 → 執行區塊
第3次迴圈：target → 元素2 → 執行區塊
```


### 二、不同序列類型的應用

#### 1. 數字範圍迭代（range）

```python
for x in range(10):
    print(x)  # 輸出 0~9
```


#### 2. 列表迭代

```python
names = ["Alice", "Bob", "Charlie"]
# Pythonic 寫法
for name in names:
    print(name)

# 非 Pythonic 寫法（避免使用）
for i in range(len(names)):
    print(names[i])
```


#### 3. 字典迭代

```python
age = {"Alice": 30, "Bob": 25, "Charlie": 35}

# 標準寫法（自動迭代鍵）
for name in age:
    print(f"{name}: {age[name]}")

# 排序鍵後迭代
for name in sorted(age, reverse=True):
    print(f"{name}: {age[name]}")
```


### 三、進階迭代技巧

| 情境 | 寫法範例 | 說明 |
| :-- | :-- | :-- |
| 同時獲取索引與元素 | `for index, value in enumerate(lst)` | 適用需要追蹤位置的場景 |
| 多序列同步迭代 | `for a, b in zip(lst1, lst2)` | 同時迭代多個等長序列 |
| 字典鍵值對迭代 | `for key, value in dict.items()` | 直接獲取鍵值對 |

### 四、While Loop 機制

- **適用場景**：當**不確定需要執行次數**時使用
- **基本結構**：

```python
while 條件式:
    # 執行區塊
else:
    # 條件為 False 時執行（可選）
```


### 五、For vs While Loop 選擇指南

| 特性 | For Loop | While Loop |
| :-- | :-- | :-- |
| 執行次數 | 已知（取決於序列長度） | 未知（取決於條件變化） |
| 典型應用 | 遍歷序列/集合元素 | 持續監測狀態（如傳感器數據） |
| 終止方式 | 自然結束或 `break` | 條件變為 False 或 `break` |
| 記憶體效率 | 需預載整個序列 | 動態評估條件，更節省資源 |


---

**最佳實踐建議**：

1. **優先使用 For Loop**：當處理已知迭代次數的場景時，程式碼更簡潔
2. **避免無限迴圈**：While Loop 務必確保條件最終會變為 False
3. **迭代器活用**：結合 `itertools` 模組處理複雜迭代邏輯
4. **效能注意**：超大數據集建議改用生成器（generator）減少記憶體消耗

**範例比較**：

```python
# For Loop 實現計數器
total = 0
for num in [1, 2, 3, 4]:
    total += num

# While Loop 實現相同功能
total = 0
i = 0
while i < 4:
    total += (i+1)
    i += 1
```

