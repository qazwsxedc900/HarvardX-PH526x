<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## Python 類別與物件導向編程（OOP）整理

### 一、類別（Class）基礎概念

- **類別定義**：用 `class` 關鍵字建立，作為物件的藍圖（blueprint）。
- **物件（實例）**：透過類別實例化（instantiate）產生的具體實體。
- **繼承（Inheritance）**：新類別可繼承現有類別的屬性和方法，並擴充功能。


#### 範例：自定義列表類別

```python
class MyList(list):  # 繼承 Python 內建 list
    def remove_min(self):
        self.remove(min(self))
    
    def remove_max(self):
        self.remove(max(self))
```


### 二、類別實作解析

1. **繼承機制**：
    - `MyList(list)` 表示繼承自內建 `list` 類別。
    - 子類別自動擁有父類別的所有方法（如 `append`, `sort`）。
2. **實例方法（Instance Methods）**：
    - 方法第一個參數必須是 `self`，指向實例本身。
    - `remove_min()` 和 `remove_max()` 是新增的自定義方法。

#### 使用範例：

```python
x = [3, 1, 4, 10, 2]
y = MyList(x)  # 實例化，y 是 MyList 的物件

y.remove_min()  # 移除最小值 1 → y = [3, 4, 10, 2]
y.remove_max()  # 移除最大值 10 → y = [3, 4, 2]
```


### 三、OOP 四大核心概念

| 概念 | 說明 | 範例應用 |
| :-- | :-- | :-- |
| **封裝** | 將數據和操作綁定在類別內，控制存取權限 | 用方法操作列表元素，隱藏內部實現細節 |
| **繼承** | 子類別繼承父類別功能，減少重複代碼 | `MyList` 繼承 `list` 的所有功能 |
| **多型** | 不同類別可實現相同方法名稱，執行不同行為 | 自定義 `remove_min()` 覆寫父類別方法（可選） |
| **抽象** | 隱藏複雜實現，只暴露必要接口 | 使用者只需呼叫 `remove_min()`，不需知道細節 |

### 四、類別設計注意事項

1. **`__init__` 方法**：
    - 用於初始化物件屬性（若未定義，會繼承父類別的 `__init__`）。
    - 範例擴充：為 `MyList` 添加日誌功能

```python
class MyList(list):
    def __init__(self, *args):
        super().__init__(*args)  # 呼叫父類別初始化
        self.operations = []  # 新增屬性記錄操作
    
    def remove_min(self):
        self.operations.append("remove_min")
        super().remove(min(self))
```

2. **方法覆寫（Override）**：
    - 子類別可重新定義父類別的方法來改變行為。
    - 範例：自定義 `sort()` 方法

```python
class MyList(list):
    def sort(self, reverse=False):
        print("自定義排序觸發！")
        super().sort(reverse=reverse)  # 仍使用父類排序邏輯
```


### 五、實務應用場景

- **擴充內建類型**：如為列表添加統計方法（平均值、標準差）。
- **封裝業務邏輯**：將資料處理流程封裝成類別方法。
- **框架開發**：透過繼承建立可複用的元件（如 Django 模型）。

---

**總結**：
類別是 Python OOP 的核心，透過繼承可高效擴充現有功能。掌握封裝、繼承、多型與抽象四大概念，能設計出結構清晰且易維護的程式。自定義類別時，務必注意 `self` 參數的使用與父類別方法的呼叫方式。

<div style="text-align: center">⁂</div>

[^1]: https://realpython.com/python3-object-oriented-programming/

[^2]: https://www.youtube.com/watch?v=Ej_02ICOIgs

[^3]: https://kinsta.com/blog/python-object-oriented-programming/

[^4]: https://www.programiz.com/python-programming/object-oriented-programming

[^5]: https://www.datacamp.com/tutorial/python-oop-tutorial

[^6]: https://www.w3schools.com/python/python_classes.asp

[^7]: https://openstax.org/books/introduction-python-programming/pages/11-1-object-oriented-programming-basics

