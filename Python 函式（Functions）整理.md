<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## Python 函式（Functions）整理

### 一、函式的用途與優點

- **函式**是用來將多個語句分組，方便在程式中多次執行。
- 透過函式可以**最大化程式碼重用**、**減少重複**。
- 有助於把大問題拆解成小單元（稱為「程序分解」）。

---

### 二、函式的定義與使用

- 使用 `def` 關鍵字定義函式。
- 用 `return` 語句將結果回傳給呼叫者。


#### 範例：加法函式

```python
def add(a, b):
    mysum = a + b
    return mysum

result = add(12, 15)  # result = 27
```


---

### 三、函式的作用域

- 在函式內建立或指派的名稱**都是區域變數**，只在函式執行時存在。
- 若要在函式內修改全域變數，需用 `global` 關鍵字。

---

### 四、參數傳遞方式

- **位置對應**：呼叫函式時，傳入的參數會依順序對應到函式的參數名稱。

```python
def mysum(a, b):
    return a + b

mysum(2, 3)  # a=2, b=3
```


---

### 五、回傳多個值

- 可以用**tuple（元組）**一次回傳多個值。


#### 範例：同時回傳加法與減法結果

```python
def add_and_sub(a, b):
    mysum = a + b
    mydiff = a - b
    return mysum, mydiff

result = add_and_sub(20, 15)  # result = (35, 5)
```


---

### 六、函式的建立與呼叫時機

- 只有當 Python 執行到 `def` 語句時，函式才會被建立。
- 函式不會自動執行，必須用「函式名稱＋括號」呼叫。
- `def` 會建立一個物件並指派給一個名稱。

---

### 七、函式名稱的指派

- 可以把同一個函式物件指派給不同名稱。

```python
def add(a, b):
    return a + b

newadd = add
print(newadd(3, 4))  # 7
```


---

### 八、參數傳遞與可變物件

- 傳遞參數時，實際上是將物件指派給函式內的區域名稱。
- 若傳入**可變物件**（如 list），函式內的修改會影響原本的物件。


#### 範例：修改列表內容

```python
def modify(mylist):
    mylist[0] *= 10

L = [1, 3, 5, 7, 9]
modify(L)
print(L)  # [10, 3, 5, 7, 9]
```


---

### 九、總結

- 函式提升程式碼重用性與可讀性。
- 可回傳單一或多個值。
- 變數作用域分為區域與全域。
- 參數傳遞時，對可變物件的修改會影響原物件。
- 可以用多個名稱指向同一個函式物件。

