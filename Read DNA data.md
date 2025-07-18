讀取檔案的最佳方式取決於您想對檔案做什麼。

*   **讀取大型檔案的部分內容：** 如果您只需要讀取檔案中的某些行，可以使用 `for` 迴圈逐行讀取、處理或跳過。這種方式節省記憶體且速度快。
*   **一次讀取整個檔案：** 如果您需要讀取檔案的全部內容，可以直接一次讀取。

在開始處理檔案之前，請確保您的 Python 工作目錄與您下載檔案的目錄一致。您可以使用 `pwd`（print working directory）指令來查看目前的工作目錄，並使用 `cd`（change directory）指令來更改目錄。

以下是如何讀取檔案的範例：

1.  **指定檔案名稱：** 例如，`input_file = "dna.txt"`
2.  **使用 `open()` 函數開啟檔案：** 例如，`f = open(input_file, "r")`，其中 `"r"` 表示讀取模式。
3.  **使用 `f.read()` 方法讀取整個檔案：** 例如，`seq = f.read()`，將檔案內容儲存在 `seq` 變數中。

讀取檔案後，您可能會發現一些額外的字元，例如 `\n`（換行符號）。這些字元可能會影響字串的顯示和後續處理。

要移除這些額外的字元，可以使用 `replace()` 方法。例如，`seq = seq.replace("\n", "")` 會將 `seq` 字串中的所有 `\n` 字元替換為空字串（即移除）。

有時，字串中可能還隱藏著其他不可見字元，例如 `\r`。為了安全起見，您可以移除這些字元，即使它們不存在也不會有任何影響。例如，`seq = seq.replace("\r", "")`。
