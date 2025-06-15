<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

# It's easy to index and slice NumPy arrays regardless of their dimension,

meaning whether they are vectors or matrices.
With one-dimension arrays, we can index a given element
by its position, keeping in mind that indices start at 0.
With two-dimensional arrays, the first index
specifies the row of the array and the second index
specifies the column of the array.
This is exactly the way we would index elements of a matrix in linear algebra.
We can also slice NumPy arrays.
Remember the indexing logic.
Start index is included but stop index is not,
meaning that Python stops before it hits the stop index.
NumPy arrays can have more dimensions than one of two.
For example, you could have three or four dimensional arrays.
With multi-dimensional arrays, you can use the colon character
in place of a fixed value for an index, which
means that the array elements corresponding
to all values of that particular index will be returned.
For a two-dimensional array, using just one index
returns the given row which is consistent with the construction of 2D
arrays as lists of lists, where the inner lists correspond
to the rows of the array.
Let's then do some practice.
I'm first going to define two one-dimensional arrays,
called lower case x and lower case y.
And I'm also going to define two two-dimensional arrays,
and I'm going to denote them with capital X
and capital Y. Let's first see how we would
access a single element of the array.
So just typing x square bracket 2 gives me the element located at position
2 of x.
I can also do slicing.
So I can specify the start index and the end index, in which case
I get two elements here from the x array, the numbers 1 and 2.
If you look at the sizes of x and y, each of them
has exactly three elements.
That means that we can add those two arrays up.
So I can type x plus y, which gives me a new array called z.
In this case, the elements of z will be element-wise additions
from the vectors x and y.
So the first element of x is added to the first element of y, and so on.
Now moving on to two-dimensional arrays,
we can also investigate individual rows or columns of arrays.
Typing X square bracket colon comma 1 gives me
access to the first column of the table X. I can do the same for Y,
and now I have to first column of the two-dimensional array, Y.
I can also add these two up.
So I can type X plus Y, again colon comma 1.
In this case, I have added together the first columns of these two arrays.
To extract the first row of X, I type, within square brackets,
1 comma colon which gives me all of the elements in the first row.
In this case, these are numbers 4, 5, and 6.
I can take also the first row of Y, and I can then add these two arrays up.
Because two-dimensional arrays are defined as nested rows,
I can use a shorthand notation to access the first row of X, which in this case
would be just X square brackets 1, and this gives me the same exact output
as typing X square bracket 1 comma colon.
One word of caution --
what happens if we take two lists and put a plus sign between them?
Well, we can give it a try.
I can define a list which consists of elements 2 and 4.
I have a plus sign followed by another list with elements 6 and 8.
Remember, putting a plus sign between two lists
concatenates those two lists, resulting in a new list which is longer than
the two lists that were added together.
Now let's look at a different example.
What happens if we first turn those lists into NumPy arrays,
and then have a plus sign between them?
I'm going to take my previous line here.
I'll just turn this into a NumPy array.
So my first NumPy array has two elements, 2 and 4.
I'm going to add that to another NumPy array, which has elements 6 and 8.
In this case, what's happening is we have two one-dimensional arrays.
And what we've accomplished here is an element-wise addition
between these two arrays.

NumPy 陣列的索引（indexing）與切片（slicing）非常靈活且直覺，無論是一維、二維還是多維陣列都能輕鬆操作。

---

## 一、基本索引

- **一維陣列（向量）**
用單一整數索引，從 0 開始。例如：

```python
import numpy as np
x = np.array([10, 20, 30, 40])
print(x[^2])  # 輸出 30
```

    - 負索引可從尾端往前數，如 `x[-1]` 取最後一個元素【1】【3】【5】【6】。
- **二維陣列（矩陣）**
用 `[row, column]` 方式索引。例如：

```python
X = np.array([[1, 2, 3], [4, 5, 6]])
print(X[1, 2])  # 輸出 6（第2列第3行，索引從0開始）
```

    - 也可單一索引取一整列：`X[^1]` 會得到 `[^4][^5][^6]`【1】【3】【5】【6】。
- **多維陣列**
用多個逗號分隔的索引。例如三維：

```python
arr = np.array([[[1,2],[3,4]], [[5,6],[7,8]]])
print(arr[1, 0, 1])  # 輸出 6
```

    - 每個索引對應一個維度【1】【5】【6】。

---

## 二、切片（Slicing）

- 語法與 list 類似：`[start:stop:step]`
    - `start` 包含，`stop` 不包含
    - 可省略 start、stop 或 step
    - 可用於每個維度

**一維切片：**

```python
x = np.array([0, 1, 2, 3, 4, 5])
print(x[1:4])    # [1, 2, 3]
print(x[:3])     # [0, 1, 2]
print(x[3:])     # [3, 4, 5]
```

**二維切片：**

```python
X = np.array([[1,2,3],[4,5,6],[7,8,9]])
print(X[1, :])   # 取第2列所有元素 [4, 5, 6]
print(X[:, 0])   # 取所有列的第1行 [1, 4, 7]
print(X[0:2, 1:3])  # 取前兩列、第2~3行 [[2,3],[5,6]]
```

- 使用冒號 `:` 代表「所有該維度元素」【4】【8】。

---

## 三、進階索引

- **布林索引**：用布林陣列選取元素
- **整數陣列索引**：用一組索引組成的陣列選取元素

```python
x = np.array([10, 20, 30, 40, 50])
idx = [1, 3]
print(x[idx])  # [20, 40]
```

- **多維索引**：可混合使用切片與整數索引

---

## 四、NumPy 陣列的加法

- **NumPy 陣列相加**：元素對元素相加

```python
a = np.array([1,2,3])
b = np.array([4,5,6])
print(a + b)  # [5, 7, 9]
```

- **Python list 相加**：串接而非數值相加

```python
a = [1,2,3]
b = [4,5,6]
print(a + b)  # [1,2,3,4,5,6]
```

    - 這是 NumPy 與 Python list 最大的差異之一【1】【5】【6】。

---

## 五、常見錯誤

- 索引超出範圍會報 `IndexError`
- 切片超出範圍不會報錯，只會返回有效部分

---

## 六、更多範例

```python
A = np.array([[1, 3], [5, 9]])
print(A[0, 1])      # 取第1列第2行，輸出3
print(A[:, 0])      # 取所有列的第1行，輸出[1, 5]
print(A[^1])         # 取第2列，輸出[5, 9]
print(A[1, :])      # 取第2列所有元素，輸出[5, 9]
```

- 三維以上索引、切片方式完全類似，只需多加一個索引維度【1】【2】【3】【5】【6】。

---

**總結：**
NumPy 陣列的索引與切片功能強大且靈活，能方便地存取、修改及運算各種維度的資料，是科學運算與數據處理的核心工具【1】【3】【5】【6】。

<div style="text-align: center">⁂</div>

[^1]: https://www.w3schools.com/python/numpy/numpy_array_indexing.asp

[^2]: https://numpy.org/devdocs/user/basics.indexing.html

[^3]: https://www.programiz.com/python-programming/numpy/array-indexing

[^4]: http://homepage.ntu.edu.tw/~weitingc/fortran_lecture/Lecture_P_3_NumPyArrayIndexing.slides.html

[^5]: https://sparkbyexamples.com/python/python-numpy-array-indexing/

[^6]: https://clouds.eos.ubc.ca/~phil/docs/problem_solving/05-NumPy-and-Arrays/05.05-Array-Indexing.html

[^7]: https://jakevdp.github.io/PythonDataScienceHandbook/02.02-the-basics-of-numpy-arrays.html

[^8]: https://www.w3schools.com/python/numpy/numpy_array_slicing.asp

[^9]: https://www.youtube.com/watch?v=eipZT4fafoM

[^10]: https://numpy.org/doc/stable/user/basics.indexing.html

[^11]: https://docs.scipy.org/doc/numpy-1.11.0/user/basics.indexing.html

[^12]: https://llego.dev/posts/numpy-array-indexing-slicing-accessing-python/

[^13]: https://betterprogramming.pub/how-to-index-data-in-python-numpy-arrays-1274ce968390?gi=db64c32660ff

