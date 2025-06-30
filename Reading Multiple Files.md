-----

## 檔案目錄導覽與書籍數據分析

在處理大量文件時，能夠在檔案目錄中導航至關重要。我們的目標是讀取書籍資料夾中各個子目錄下包含的每一本書。

### 導入與設定

首先，我們需要導入 `os` 模組來進行目錄操作。同時，我們定義一個變數 `book_dir` 來儲存書籍的主目錄路徑。

import os
from collections import Counter
import pandas as pd # 導入 pandas 庫

# 假設這些是前面定義的函數
def read_book(title_path):
    """
    讀取一本書並將其內容作為字串返回。
    替換換行符 ('\n') 和回車符 ('\r') 為空字串，確保文字連續。
    """
    with open(title_path, 'r', encoding='utf8') as current_file:
        text = current_file.read()
    text = text.replace('\n', '').replace('\r', '')
    return text

def count_words_fast(text):
    """
    快速計算文本中單詞的頻率。
    將文本轉換為小寫並跳過常見標點符號。
    使用 collections.Counter 實現高效計數。
    """
    text = text.lower()
    skip_chars = ['.', ',', ';', ':', "'", '"']
    for ch in skip_chars:
        text = text.replace(ch, '')
    word_counts = Counter(text.split())
    return word_counts

def word_stats(word_counts):
    """
    返回獨特單詞的數量和單詞頻率。
    輸入參數 word_counts 應為字典或 Counter 物件。
    """
    num_unique = len(word_counts)
    counts = word_counts.values()
    return num_unique, counts

# 設定書籍目錄路徑
book_dir = "./Books" # 假設您的書籍資料夾在此路徑

# 檢查目錄內容
# print(os.listdir(book_dir)) # 這將顯示 'English', 'French', 'German', 'Portuguese' 等子目錄

### 遍歷多層目錄

我們的目標是遍歷 `book_dir` 下的所有語言目錄，然後是作者目錄，最後是每本書的標題文件。這需要三個嵌套的 `for` 迴圈。

1.  **第一層：語言目錄**
    遍歷 `book_dir` 中的所有語言資料夾。

for language in os.listdir(book_dir):
        # ...

2.  **第二層：作者目錄**
    在每個語言資料夾內，遍歷其下的所有作者資料夾。我們需要將 `book_dir`、斜線 `/` 和 `language` 串聯起來，以形成完整的路徑。

for language in os.listdir(book_dir):
        for author in os.listdir(os.path.join(book_dir, language)): # 使用 os.path.join 更好
            # ...

3.  **第三層：書本標題**
    在每個作者資料夾內，遍歷所有書本文件。同樣，我們需要構建完整的路徑：`book_dir + "/" + language + "/" + author`，然後再拼接上書本的標題。

for language in os.listdir(book_dir):
        for author in os.listdir(os.path.join(book_dir, language)):
            for title in os.listdir(os.path.join(book_dir, language, author)): # 使用 os.path.join 更好
                # 構建完整的書籍文件路徑
                input_file = os.path.join(book_dir, language, author, title)
                # print(input_file) # 可以列印路徑以進行驗證
                # ...

### 整合書籍數據處理

現在，我們將 `read_book`、`count_words_fast` 和 `word_stats` 函數整合到這個三重迴圈中，以處理每一本書。

# 初始化一個空的 pandas DataFrame 來儲存統計數據
# 我們將定義以下欄位：語言、作者、書名、長度（總詞數）、獨特詞數
stats = pd.DataFrame(columns=['language', 'author', 'title', 'length', 'unique'])
title_num = 0 # 用於追蹤行數的索引

for language in os.listdir(book_dir):
    for author in os.listdir(os.path.join(book_dir, language)):
        for title in os.listdir(os.path.join(book_dir, language, author)):
            input_file = os.path.join(book_dir, language, author, title)
            
            # 讀取書籍文本
            text = read_book(input_file)
            
            # 計算單詞頻率
            word_counts = count_words_fast(text)
            
            # 獲取單詞統計數據
            num_unique, counts = word_stats(word_counts)
            
            # 計算總詞數
            total_words = sum(counts)
            
            # 將數據添加到 DataFrame 中
            # 為了美化輸出，我們對作者名進行首字母大寫，並移除書名中的 .txt 副檔名
            stats.loc[title_num] = [
                language,
                author.capitalize(), # 作者名首字母大寫
                title.replace('.txt', ''), # 移除 .txt 副檔名
                total_words,
                num_unique
            ]
            title_num += 1 # 增加行數計數器

# 運行程式碼後，您就可以查看 stats 表格了
# print(stats.head()) # 查看前 5 行
# print(stats.tail()) # 查看後 5 行
# print(stats.shape) # 查看表格的形狀 (行數, 列數)

### 使用 Pandas 儲存統計數據

為了更好地組織和分析從每本書中提取的數據，我們將使用 `pandas` 庫。`pandas` 提供 `DataFrame` 這種強大的數據結構，非常適合處理表格數據。

1.  **導入 Pandas 並創建空的 DataFrame**：
    首先，我們導入 `pandas` 並按照慣例命名為 `pd`。然後，我們創建一個空的 `DataFrame`，並定義其列名，例如 `"language"`、`"author"`、`"title"`、`"length"`（總詞數）和 `"unique"`（獨特詞數）。

import pandas as pd
    # ... (之前的函數定義)

    stats = pd.DataFrame(columns=['language', 'author', 'title', 'length', 'unique'])
    title_num = 0 # 用於跟蹤表格中的行號

2.  **向 DataFrame 中添加數據**：
    在三重迴圈的末尾，我們將每本書的統計數據添加到 `stats` 這個 `DataFrame` 中。我們使用 `stats.loc[title_num] = [...]` 的語法來指定行號並賦值。同時，每次添加完一行數據後，務必將 `title_num` 增加 1。

    為了使表格更整潔，我們還會做一些小改動：

      * 將作者名統一**首字母大寫**：`author.capitalize()`。
      * 移除書名中的 `.txt` **文件副檔名**：`title.replace('.txt', '')`。

### 查看結果

執行上述程式碼後，`stats` 這個 `DataFrame` 將包含所有書籍的統計數據。由於表格可能很大，我們可以只查看表格的開頭或結尾：

  * `stats.head()`：顯示表格的前 5 行。
  * `stats.tail()`：顯示表格的後 5 行。
  * `stats.shape`：顯示表格的行數和列數。

這讓我們可以方便地驗證數據是否正確地填充到表格中，並且作者名已大寫，書名中已去除 `.txt` 副檔名。

-----
