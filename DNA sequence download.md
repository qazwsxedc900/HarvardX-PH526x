這個任務是要手動從 NCBI (美國國家生物技術資訊中心) 下載 DNA 序列和對應的蛋白質序列，NCBI 是美國主要的 DNA 和相關資訊公共儲存庫。

步驟如下：

進入 NCBI 網站： 可以直接前往 NCBI 網站，或者在 Google 搜尋 "NCBI"，通常第一個結果就是。
選擇 Nucleotide 資料庫： 在 NCBI 網站頂部，找到 "All Databases" (所有資料庫)，然後向下滾動，找到 "Nucleotide" (核苷酸) 並選擇它。
輸入搜尋代碼： 在搜尋框中輸入 NM_207618.2 (注意大小寫和底線)。
搜尋並確認結果： 點擊 "Search" (搜尋)。如果搜尋正確，你會看到包含特定樣本資訊的頁面。
下載 DNA 序列：
在頁面頂部點擊 "FASTA"。
用滑鼠選取從 "G" 開始到 "T" 結束的整個 DNA 序列。
複製選取的序列。
打開 Spyder 編輯器，新建一個檔案。
將複製的 DNA 序列貼上到新檔案中。
儲存檔案，命名為 dna.txt，並選擇儲存為 text file (文字檔案) 到你的 python_case_studies 資料夾下的 translation 子資料夾中。
下載蛋白質序列：
回到 NCBI 頁面。
點擊 "CDS" (編碼序列)。
在彈出視窗的右下角，找到 "translation" (轉譯) 的蛋白質序列。
選取整個蛋白質序列並複製它。
回到 Spyder 編輯器，新建一個檔案。
將複製的蛋白質序列貼上到新檔案中。
儲存檔案，命名為 protein.txt，並選擇儲存為 text file (文字檔案) 到你的 python_case_studies 資料夾下的 translation 子資料夾中。
現在你就成功下載了 DNA 序列 (dna.txt) 和蛋白質序列 (protein.txt)，可以開始後續的分析工作。
