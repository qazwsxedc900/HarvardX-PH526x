<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## 隨機性與 Python 隨機模擬整理

### 一、隨機性在自然與建模中的角色

- 自然界與社會現象中常常存在隨機性（randomness），例如分子運動、選舉民調等。
- 在建模複雜系統時，無法解釋的部分常以「隨機」處理，把無法簡單建模的部分視為雜訊（noise）。
- 所以學會用 Python 產生隨機數、模擬隨機過程非常重要。

---

### 二、Python 隨機模組與基本用法

- Python 的 `random` 模組可用來模擬各種隨機事件。
- **擲硬幣**：

```python
import random
result = random.choice(['H', 'T'])  # 隨機回傳 'H' 或 'T'
```

- 也可用數字 0/1 表示：

```python
result = random.choice([0, 1])  # 隨機回傳 0 或 1
```


---

### 三、模擬擲骰子

- 一顆六面骰：

```python
result = random.choice([1, 2, 3, 4, 5, 6])
# 或用 range
result = random.choice(range(1, 7))  # 1~6
```

- 注意：`range(1, 7)` 產生 1~6，因為 range 的結尾不包含 stop。

---

### 四、常見錯誤提醒

- 若寫成 `random.choice([range(1, 7)])`，會回傳「range 物件」而不是數字，因為 list 裡只有一個元素（range 物件本身）。
- 正確用法是直接給一個數字序列，而不是把 range 包在一個 list 裡。

---

### 五、進階範例：隨機選擇多種骰子

- 假設有三種骰子：6 面、8 面、10 面，隨機選一顆骰子再擲一次：

```python
dice_list = [range(1, 7), range(1, 9), range(1, 11)]
chosen_dice = random.choice(dice_list)  # 隨機選一個 range
result = random.choice(chosen_dice)     # 再從該 range 隨機選一個數字
```

- 這樣可以模擬「先選骰子、再擲骰子」的複合隨機過程。

---

### 六、重點整理

1. **隨機性是建模與現象分析的重要工具**。
2. **random.choice** 可用於任何序列（list、tuple、range）。
3. **range** 是一種序列，可以直接作為 random.choice 的參數。
4. **巢狀 random.choice** 可模擬複雜隨機過程（如多種骰子）。
5. **小心參數型態**，避免將 range 包在 list 裡只產生一個物件。

---

這些技巧能幫助你用 Python 有效模擬各種隨機現象與抽樣實驗，無論是科學、統計還是遊戲設計都非常實用。

