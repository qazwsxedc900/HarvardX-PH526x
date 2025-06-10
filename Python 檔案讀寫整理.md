<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## Python 檔案讀寫整理

### 一、讀取檔案

1. **建立檔案名稱變數**

```python
filename = "input.txt"
```

2. **用 for 迴圈逐行讀取檔案**

```python
for line in open(filename):
    print(line)
```

    - `open(filename)` 會產生一個檔案物件，可以直接用 for 迴圈逐行讀取。
    - 每次迴圈，`line` 變數會取得檔案中的一行字串（包含換行符號）。
3. **去除行尾換行符號**

```python
for line in open(filename):
    line = line.rstrip()  # 去除右側的換行或空白
    print(line)
```

    - `rstrip()` 方法會移除字串右側的空白或換行符號，避免多餘的空行。
4. **分割每一行內容**

```python
for line in open(filename):
    line = line.rstrip()
    parts = line.split()  # 以空白分割，回傳一個列表
    print(parts)
```

    - `split()` 方法會將字串依指定分隔符（預設為空白）切割，產生一個列表。

---

### 二、寫入檔案

1. **開啟檔案以寫入模式**

```python
F = open("output.txt", "w")  # "w" 代表寫入模式
```

2. **寫入內容**

```python
F.write("Python\n")  # 記得加換行符號 "\n"
```

3. **關閉檔案**

```python
F.close()
```


---

### 三、重點整理

- **讀檔時**，每一行都包含換行符號，可用 `rstrip()` 去除。
- **字串是不可變物件**，使用 `rstrip()` 需重新指派給變數。
- **分割字串**可用 `split()`，回傳的是一個列表。
- **寫檔時**，需指定寫入模式 `"w"`，寫入後要記得 `close()` 關閉檔案。
- **寫入多行時**，每行結尾需加 `\n` 換行符號。

---

### 四、範例總結

**讀檔範例：**

```python
for line in open("input.txt"):
    print(line.rstrip().split())
```

**寫檔範例：**

```python
F = open("output.txt", "w")
F.write("Python\n")
F.close()
```

這些就是 Python 讀寫文字檔案的基本方法！

