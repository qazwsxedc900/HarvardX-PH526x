<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## Python 的作用域規則（Scope Rules）與 LEGB 原則

Python 中變數與函式名稱的解析，遵循「LEGB」規則，這決定了在程式不同位置、同名變數或函式時，Python 會使用哪一個物件[^2][^6][^1]。

### LEGB 原則說明

- **L（Local）本地作用域**
指目前函式（或 lambda 表達式）內部建立的名稱，只能在該函式內部使用[^2][^1][^6]。
- **E（Enclosing）包裹作用域**
僅出現在巢狀函式中，指外層函式的本地作用域。內層函式可存取外層函式定義的變數[^2][^4]。
- **G（Global）全域作用域**
指模組層級（即整個檔案）定義的名稱，可以在該模組內任何地方存取[^2][^1][^6]。
- **B（Built-in）內建作用域**
Python 內建的名稱（如 len、str 等），所有程式都能存取[^2][^6]。

Python 會依序從 Local → Enclosing → Global → Built-in 這四層尋找名稱，一旦找到就停止搜尋[^2][^6]。

---

### 範例解析

#### 1. 基本範例

```python
x = 300  # 全域變數

def myfunc():
    x = 200  # 區域變數
    print(x) # 輸出 200

myfunc()
print(x)     # 輸出 300
```

- `myfunc()` 內的 `x` 是區域變數，只在該函式內有效[^1][^4]。
- 函式外的 `x` 是全域變數。


#### 2. 修改全域變數

若要在函式內修改全域變數，需使用 `global` 關鍵字[^1][^3][^4]：

```python
x = 300

def myfunc():
    global x
    x = 200

myfunc()
print(x)  # 輸出 200
```


#### 3. 巢狀函式與 nonlocal

巢狀函式可用 `nonlocal` 關鍵字修改外層函式變數[^1][^4]：

```python
def outer():
    x = "local"
    def inner():
        nonlocal x
        x = "nonlocal"
    inner()
    print(x)  # 輸出 nonlocal

outer()
```


---

### 作用域規則的實際應用與常見問題

- **同名變數/函式**：Python 依 LEGB 規則由內而外逐層尋找，找到第一個就使用[^2][^6]。
- **NameError**：若所有層級都找不到該名稱，會拋出 NameError，程式中止[^2][^6]。
- **副作用（side effect）**：函式內部若直接修改全域變數或可變物件，會產生副作用，建議避免這種寫法以提升程式可維護性。


#### 例子說明

```python
def update():
    x.append(1)  # x 在本地找不到，往外層找

x = [1, 1]
update()
print(x)  # 輸出 [1, 1, 1]
```

- `update()` 內沒有本地的 `x`，會往全域尋找，找到全域的 `x` 並修改它[^2][^1]。

---

### 小結

- **作用域（scope）** 決定變數/函式名稱的可見範圍。
- **LEGB 原則**：Local → Enclosing → Global → Built-in，逐層尋找名稱。
- **副作用與可維護性**：避免函式內部直接修改全域變數或可變物件，減少程式錯誤風險[^9]。

理解與運用好 Python 的作用域規則，是寫出正確、可維護程式的基礎[^2][^6][^1]。

<div style="text-align: center">⁂</div>

[^1]: https://www.w3schools.com/python/python_scope.asp

[^2]: https://realpython.com/python-scope-legb-rule/

[^3]: https://stackoverflow.com/questions/291978/short-description-of-the-scoping-rules

[^4]: https://www.programiz.com/python-programming/global-local-nonlocal-variables

[^5]: https://www.youtube.com/watch?v=KyCw1uA1-M8

[^6]: https://www.datacamp.com/tutorial/scope-of-variables-python

[^7]: https://labex.io/tutorials/python-how-to-manage-python-scope-rules-421901

[^8]: https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Scope.html

[^9]: programming.python

