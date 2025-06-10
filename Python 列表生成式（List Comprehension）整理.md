<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## Python 列表生成式（List Comprehension）整理

### 一、列表生成式的用途

在 Python 中，經常需要將一個現有的列表進行某種運算後，產生一個新列表。
這種需求可以用「列表生成式」來快速、簡潔地完成。

---

### 二、傳統寫法（for 迴圈）

假設有一個數字列表 `numbers`，想要得到每個數字的平方，可以這樣寫：

```python
numbers = [1, 2, 3, 4, 5]
squares = []
for number in numbers:
    square = number ** 2
    squares.append(square)
# squares = [1, 4, 9, 16, 25]
```


---

### 三、列表生成式寫法

同樣的功能，用列表生成式只需一行：

```python
squares2 = [number ** 2 for number in numbers]
# squares2 = [1, 4, 9, 16, 25]
```


---

### 四、列表生成式的語法結構

```
[運算式 for 變數 in 可迭代對象]
```

- 運算式：對每個元素要做的操作（例如 `number ** 2`）
- 變數：每次迭代時代表序列中一個元素的名稱
- 可迭代對象：如列表、range、字典等

---

### 五、為什麼要用列表生成式？

1. **速度快**：列表生成式在 Python 中運行效率高於傳統 for 迴圈加 append。
2. **語法簡潔**：一行即可完成複雜操作，程式碼更易讀、更優雅。

---

### 六、進階用法（條件篩選）

還可以加上條件，篩選出想要的元素：

```python
even_squares = [number ** 2 for number in numbers if number % 2 == 0]
# even_squares = [4, 16]
```


---

### 七、總結

- 列表生成式是 Python 處理列表運算的高效、簡潔工具。
- 推薦在需要對列表元素批次處理時優先考慮使用列表生成式。

