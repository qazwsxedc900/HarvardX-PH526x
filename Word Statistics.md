單詞統計函數：word_stats
給定一個字典或 collections 模組中的 Counter 物件，我們需要一個函數來執行以下操作：

計算一本書中獨特單詞的數量。

返回每個單詞的頻率，即每個單詞出現的次數。

為此，我們將編寫一個名為 word_stats (單詞統計的縮寫) 的函數。此函數的輸入將是 word_counts 物件，該物件由我們之前編寫的單詞計數函數返回。

函數定義
from collections import Counter
import os # 引入 os 模組用於路徑操作

def read_book(title_path):
    """
    讀取一本書並將其內容作為字串返回。
    替換換行符 ('\n') 和回車符 ('\r') 為空字串，確保文字連續。
    """
    with open(title_path, 'r', encoding='utf8') as current_file:
        text = current_file.read()
    # 替換換行符和回車符
    text = text.replace('\n', '').replace('\r', '')
    return text

def count_words_fast(text):
    """
    快速計算文本中單詞的頻率。
    將文本轉換為小寫並跳過常見標點符號。
    使用 collections.Counter 實現高效計數。
    """
    text = text.lower() # 轉換為小寫
    
    # 定義要跳過的標點符號
    skip_chars = ['.', ',', ';', ':', "'", '"']
    for ch in skip_chars:
        text = text.replace(ch, '') # 將標點符號替換為空字串
    
    # 使用 Counter 對象高效計數
    word_counts = Counter(text.split())
    return word_counts

def word_stats(word_counts):
    """
    返回獨特單詞的數量和單詞頻率。
    輸入參數 word_counts 應為字典或 Counter 物件。
    """
    # 計算獨特單詞的數量
    num_unique = len(word_counts)
    
    # 提取每個單詞的頻率 (計數)
    counts = word_counts.values()
    
    # 返回包含獨特單詞數量和頻率的元組
    return num_unique, counts


功能說明
計算獨特單詞數量：
我們可以使用 word_counts 物件，直接獲取其長度 (len(word_counts))。我們知道字典中的每個條目都是唯一的，因此該物件的長度將返回我們擁有的唯一鍵的數量，即獨特單詞的數量。我們將這個數字命名為 num_unique。這完成了第一個任務。

返回單詞頻率：
第二個任務是返回字典中每個獨特單詞的計數。我們從 word_counts 字典開始，並使用 .values() 方法來提取計數器，也就是文本中每個單詞的頻率。我們將其命名為 counts。

返回結果：
最後，我們需要返回這兩個物件。我們將返回一個元組，其中包含 num_unique 和 counts。

函數測試與應用
讓我們實際執行我們的函數。首先，我們讀取書籍文本，然後計算單詞，最後使用 word_stats 來獲取統計數據。

# 假設我們有這些路徑，實際運行時請替換為您本地文件的正確路徑
# 例如： 'data/shakespeare/english/romeo-and-juliet.txt'
# 請確保 'data' 目錄存在且包含所需的書籍文件
english_book_path = 'data/shakespeare/english/romeo-and-juliet.txt'
german_book_path = 'data/shakespeare/german/romeo-und-julia.txt'

# 讀取英文版羅密歐與茱麗葉
try:
    text_en = read_book(english_book_path)
    # 計算英文版單詞頻率
    word_counts_en = count_words_fast(text_en)
    # 獲取英文版單詞統計
    num_unique_en, counts_en = word_stats(word_counts_en)

    print("--- 英文版《羅密歐與茱麗葉》統計 ---")
    print(f"獨特單詞數量 (英文): {num_unique_en}")
    # 計算總詞數 (所有單詞頻率之和)
    total_words_en = sum(counts_en)
    print(f"總單詞數量 (英文): {total_words_en}")
    print("-" * 40)

    # 讀取德文版羅密歐與茱麗葉
    text_de = read_book(german_book_path)
    # 計算德文版單詞頻率
    word_counts_de = count_words_fast(text_de)
    # 獲取德文版單詞統計
    num_unique_de, counts_de = word_stats(word_counts_de)

    print("--- 德文版《羅密歐與茱麗葉》統計 ---")
    print(f"獨特單詞數量 (德文): {num_unique_de}")
    # 計算總詞數 (所有單詞頻率之和)
    total_words_de = sum(counts_de)
    print(f"總單詞數量 (德文): {total_words_de}")
    print("-" * 40)

    print("\n--- 比較結果 ---")
    print(f"英文版獨特單詞數量: {num_unique_en}")
    print(f"德文版獨特單詞數量: {num_unique_de}")
    print(f"英文版總單詞數量: {total_words_en}")
    print(f"德文版總單詞數量: {total_words_de}")

except FileNotFoundError:
    print(f"錯誤：未能找到文件。請確保路徑正確且文件存在：")
    print(f"  英文路徑: {english_book_path}")
    print(f"  德文路徑: {german_book_path}")
    print("您可能需要將 'data' 目錄及其內容放在與您的 Python 腳本相同的目錄中。")
except Exception as e:
    print(f"處理文件時發生錯誤: {e}")


結果分析
運行上述程式碼，我們可以看到：

英文版《羅密歐與茱麗葉》：

獨特單詞數量：大約 5118 個。

總單詞數量：大約 41,000 個。

德文版《羅密歐與茱麗葉》：

獨特單詞數量：約 7,500 個。

總單詞數量：約 20,000 個。

從結果來看，德文翻譯版的獨特單詞數量似乎更多（約 7,500 個），但總詞數卻較少（約 20,000 個），這與英文版的情況（總詞數約 41,000 個）有所不同。這可能是由於語言結構、翻譯風格或原文與譯文在詞彙使用上的差異造成的。
