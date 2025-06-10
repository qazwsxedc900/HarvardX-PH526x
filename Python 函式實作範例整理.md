<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## Python 函式實作範例整理

### 一、交集函式 intersect

**功能說明**
此函式接收兩個序列（如 list），回傳兩者的交集元素（即同時存在於兩個序列中的元素）。

**程式碼範例：**

```python
def intersect(s1, s2):
    res = []
    for x in s1:
        if x in s2:
            res.append(x)
    return res
```

**使用方式：**

```python
list1 = [1, 2, 3, 4, 5]
list2 = [3, 4, 5, 6, 7]
print(intersect(list1, list2))  # 輸出: [3, 4, 5]
```


---

### 二、隨機密碼產生函式 password

**功能說明**
此函式根據指定長度，從指定字元集合中隨機選取字元組成密碼。

**程式碼範例：**

```python
import random

def password(length):
    pw = ""
    characters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
    for i in range(length):
        pw += random.choice(characters)
    return pw
```

**使用方式：**

```python
print(password(4))   # 可能輸出: "aB3d"
print(password(10))  # 可能輸出: "Gh2kLm9QwZ"
```


---

### 三、函式撰寫與呼叫重點

- 使用 `def` 關鍵字定義函式，函式名稱後加小括號與參數，最後加冒號，主體需縮排[^1][^2][^3][^5]。
- 參數可依需求設計，呼叫時將實際值傳入。
- 可用 `return` 回傳結果，若回傳多個值會自動組成 tuple。
- 函式僅在被呼叫時執行，定義時不會自動執行[^1][^2][^5]。
- 可以將同一個函式物件指派給不同名稱。

---

### 四、進階補充

- 可用 `random.choice()` 從字串或列表中隨機選取一個元素。
- 字串可用 `+` 運算子進行串接。
- 若要擴充密碼字元集，只需修改 `characters` 內容即可。

---

### 五、記憶重點

- 函式可提升程式重用性與結構清晰度。
- 輸入參數可靈活設計，回傳值可單一或多個。
- 利用內建模組（如 random）可快速實現隨機功能[^8]。

---

**總結：**
透過函式的設計與呼叫，可以有效將重複邏輯封裝，提升 Python 程式的可讀性與維護性。隨機密碼產生、序列交集等常見任務都可輕鬆以函式實現。

<div style="text-align: center">⁂</div>

[^1]: https://www.w3schools.com/python/python_functions.asp

[^2]: https://cs.stanford.edu/people/nick/py/python-function.html

[^3]: https://www.tutorialsteacher.com/python/python-user-defined-function

[^4]: https://docs.python.org/3/library/functions.html

[^5]: https://www.simplilearn.com/tutorials/python-tutorial/python-functions

[^6]: https://www.programiz.com/python-programming/function-argument

[^7]: https://yasirbhutta.github.io/python/docs/functions/

[^8]: programming.python

