<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## Python 常見錯誤整理

### 1. 未理解或忽略錯誤訊息

- **範例：list index out of range**
    - 當你嘗試存取超出列表索引範圍的元素時，會出現 `IndexError: list index out of range`[^1][^2][^3][^5][^6][^7]。
    - 例如，列表長度為 3（索引為 0, 1, 2），若存取 L[^3] 或 L[^5] 都會報錯。
    - 解決方式：在存取前確認索引是否在 `0 ~ len(L)-1` 範圍內。


### 2. 忘記字典（dict）本身無順序

- 字典的 key-value 配對沒有固定順序，不能假設其排列方式。
- 存取時要根據 key，而不是依序號或位置。


### 3. 誤用物件方法

- **範例：list 沒有 add 方法**
    - 對 list 使用 `L.add(8)` 會出現 `AttributeError`，正確方法應為 `L.append(8)`。
    - 解決方式：確認物件型別及支援的方法。


### 4. 錯用字典 key 型別

- **範例：KeyError**
    - 若字典的 key 是字串 `"1"`，但你用數字 `1` 查詢，會出現 `KeyError: 1`。
    - 解決方式：確認查詢時 key 的型別與字典一致。


### 5. 嘗試修改不可變物件

- **範例：字串不可變**
    - 字串（str）是不可變物件，不能直接修改其內容（如 `s = "P"` 會報錯）。
    - 解決方式：若需修改，必須建立新字串。


### 6. 不同型別間操作

- **範例：字串與數字串接**
    - `"the answer is" + 8` 會出現 `TypeError`，因為不能直接串接字串與數字。
    - 解決方式：先將數字轉為字串：`"the answer is" + str(8)`。


### 7. 縮排錯誤（Indentation Error）

- Python 的縮排決定程式邏輯結構。
- 常見錯誤：`return` 放在 for 迴圈內，導致只執行一次就結束。
    - 應將 `return` 放在 for 迴圈外層，確保整個迴圈執行完畢才回傳結果。

---

**重點提醒：**

- 熟悉常見錯誤訊息（如 IndexError、KeyError、AttributeError、TypeError、IndentationError），能快速定位問題。
- 學會用 `len()`、`range()` 等內建函式避免索引錯誤[^1][^6]。
- 任何時候都要確認操作物件的型別與支援的方法[^8]。
- 注意 Python 的縮排規則，避免邏輯錯誤。

這些都是 Python 初學者常見的錯誤與解決方式，熟悉後能大幅提升除錯效率。

<div style="text-align: center">⁂</div>

[^1]: https://rollbar.com/blog/python-indexerror/

[^2]: https://stackoverflow.com/questions/1098643/does-indexerror-list-index-out-of-range-when-trying-to-access-the-nth-item-m

[^3]: https://blog.csdn.net/Bit_Coders/article/details/114653732

[^4]: https://www.reddit.com/r/learnpython/comments/102tnf7/indexerror_list_index_out_of_range_not_sure_how/

[^5]: https://rollbar.com/blog/how-to-fix-python-list-index-out-of-range-error-in-for-loops/

[^6]: https://opensource.com/article/23/1/fix-indexerror-python

[^7]: https://www.codecademy.com/forum_questions/524e0750f10c60448f001750

[^8]: programming.python

