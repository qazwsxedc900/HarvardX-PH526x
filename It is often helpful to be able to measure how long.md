<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

# It is often helpful to be able to measure how long a segment of code

takes to run.
One reason for wanting to know might be that you
have two or more ways of coding up the same task
and you'd like to know which one is faster.
Another reason might be that you have a large dataset
and you'd like to have a sense ahead of time
how long it might take to run your code.
You could do this by running the code for smaller datasets first, time the
running time, and then extrapolate from there to the running
time for the whole dataset.
A simple way to measure time in Python is to use the time module.
We will import the module by saying import time.
One of the most useful functions in the time module is the time.clock function.
That gives us the current time.
We can capture that in a variable called start time.
So we just type start_time equals time.clock.
And then we can run this again to extract the end time.
I'm going to call that end time.
So end_time equals time.clock.
If I want to see how much time has elapsed between these two time points,
I can take my end time and from that, I will subtract my start time.
There are a few different ways to measure time in Python and each of them
has its uses.
But for many purposes, such as comparing performance,
time.clock is the right function to use.
Working with our previous example, the one that relied only on pure Python,
I have the code here in front of me.
In order to time its performance, I'm first
going to catch the start time by saying start_time equals time.clock.
The code will run
and once it's finished, I went to capture the end time.
So I capture time here again.
And then what I'd like to do is print the difference between end time
and start time.
Let's try running this code.
In this case, we might expect this to take somewhere between 5 seconds
and perhaps up to 1 minute, depending on the speed of your computer.
Let's then look at the second implementations of this example,
the one that relies on NumPy.
Again, we will capture start time,
and will the same once the code has run.
In this case, we just have two lines for the code.
And in the end, we just want to print out the difference between end time
and start time.
We would expect this code to run much faster because it makes use of NumPy.
In this case, the code runs almost immediately.
Let's look at the difference in performance
between these two different ways of implementing the same example.
I'm going to take the time I got from the first example, the pure Python
implementation, and I will then divide that by the time I got using NumPy.
In this case, we see that the NumPy implementation is over 80 times faster
than the Python-based implementation.

在 Python 中，測量程式碼執行時間很重要，例如比較不同寫法的效能，或預估處理大型資料集所需時間。最簡單的方式之一是使用 `time` 模組。

### 基本用法

1. **匯入 time 模組**

```python
import time
```

2. **記錄開始與結束時間**

```python
start_time = time.clock()  # 開始時刻
# ... 執行你的程式碼 ...
end_time = time.clock()    # 結束時刻
elapsed = end_time - start_time
print("執行時間:", elapsed)
```

    - 這種方式可以測量一段程式碼的執行時間，適合效能比較或預估運算時間【3】【5】【8】。

### 注意事項

- `time.clock()` 的行為**依平台不同**：
    - 在 UNIX 系統，回傳 CPU 處理時間（不計 sleep 等非運算時間）。
    - 在 Windows，回傳自程式啟動以來的「牆鐘時間」（wall-clock time）【2】【3】【5】【8】。
- **Python 3.3 之後**，`time.clock()` 已被棄用，建議用 `time.perf_counter()` 或 `time.process_time()` 取代，這些方法更精確且跨平台一致【2】【5】【7】【8】。


### 進階替代方法

- `time.time()`：回傳自 epoch（1970/1/1）以來的秒數，適合測量經過的真實時間（wall time），但精度有限【2】【5】【6】【7】。
- `time.perf_counter()`：高精度計時，適合效能測試。
- `time.process_time()`：只計算 CPU 使用時間，不計入 sleep 等非運算時間。


### 實際應用

- 比較純 Python 寫法與 NumPy 寫法的運算速度時，常用上述方法計時，通常 NumPy 會快很多倍。
- 可先用小型資料測試，推估大資料的運算時間。

---

**總結：**

- 用 `time` 模組可簡單測量程式片段執行時間。
- `time.clock()` 已被棄用，建議改用 `time.perf_counter()` 或 `time.process_time()`。
- 測試效能時，記得考慮不同方法的精確度與適用情境【2】【3】【5】【7】【8】。

<div style="text-align: center">⁂</div>

[^1]: https://docs.python.org/3/library/time.html

[^2]: https://stackoverflow.com/questions/85451/pythons-time-clock-vs-time-time-accuracy

[^3]: https://www.tutorialspoint.com/python/time_clock.htm

[^4]: https://www.reddit.com/r/learnpython/comments/ybdv2a/how_does_the_timeclock_function_work/

[^5]: https://linuxhint.com/python-time-clock-method/

[^6]: https://pymotw.com/2/time/index.html

[^7]: https://pymotw.com/3/time/index.html

[^8]: https://stackoverflow.com/questions/25527423/confused-about-the-behavior-of-time-clock-in-python

[^9]: https://osdoc.cogsci.nl/3.2/manual/python/clock/

[^10]: https://www.programiz.com/python-programming/time

[^11]: https://builtin.com/articles/timing-functions-python

[^12]: https://discuss.pytorch.org/t/why-time-time-in-python-is-inaccurte/94274

[^13]: https://discuss.python.org/t/is-there-any-way-to-set-parameters-for-the-clock-module-or-do-i-have-to-use-a-different-one/29828

[^14]: https://docs.python.org/uk/dev/c-api/time.html

[^15]: https://stackoverflow.com/questions/72650080/how-do-i-implement-the-clock-method-in-python-using-the-imports-time-library

[^16]: https://codeinu.net/language/python/c1986902-clock-function-in-python-time-module

