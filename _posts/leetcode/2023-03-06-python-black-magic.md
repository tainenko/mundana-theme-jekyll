---
layout: post
title:  "[Leetcode] Python刷題黑魔法"
author: tony
categories: [leetcode, python]
image: assets/images/leetcode-python.jpeg
tags: [leetcode,python]
crosspost_to_medium: true
---
分享一些 Python3 刷 Leetcode 的小技巧，善用這些技巧可以讓code變
得更簡潔，也可以讓解題過程更加舒適潤滑，當然這些技巧有人喜歡，也有人
不喜歡，姑且稱之它為黑魔法吧。

# 獲取List中第一個滿足條件的元素
`next` 搭配 `generator comprehension` 可以獲取 list 中第一個滿足條件的元素，例如以下的例子會拿到 arr 中的第一個正數
```python
# 拿到第一個正數
next(ele for ele in arr if ele > 0)
```
要注意的是當arr中的元素都不符合條件時，會raise `StopIteration`，因此在符合條件的元素可能不存在的場景要慎用。

# 對稱反轉
假如題目有兩個以上的參數，且參數的大小順序會影響解題過程時，可以在函數的開頭先做一次檢查，確保
在解題過程中冗餘且重複的參數判斷。  

舉個例子，以下的寫法可以確保解題過程中，一定滿足 a <= b。
```python
def foo(a, b):
    if a > b: return foo(b, a)
    ...
```
另一個做法是直接在函數的入口處swap參數
```python
def foo(a, b):
    if a > b: a, b = b, a
    ...
```

#  flatten dict
在python的dict，key可以使用tuple，因此在巢狀dict的情境，可以用tuple組成composition key的方式，像是 `d[k1, k2, k3]` 
簡化巢狀dict `d[k1][k2][k3]`

# Packing & Unpacking
Packing & Unpacking 可以用來取出需要的參數，例如：
```python
arr = [1, 2, 3, 4, 5]

a, *inter, b = arr
# a == 1
# inter == [2, 3, 4]
# b == 5
可以得到 foo == 1, B == [2, 3, 4], bar == 5
```
另外還可以用巢狀 unpacking， 例如以下的unpacking在平面座標問題時很常用，
```python
# dirs = [(x1, y1), (x2, y2), ...]
for idx, (x, y) in enumerate(dirs):
    ...
```
# early return
在某些問題，input可能為空值，且當input為空值時會影響函數的執行時，可以利用early return的方式處理。  
例如，linked list，tree，matrix等都有可能遇到輸入為空值的情況。當linked list的node為None時，如果繼續進
行以下的node.right或node.left時就會出現exception，因此要先判斷node是否可以操作。

```python

def foo(input):
    if not input:
        return ...
```

另外，list或是matrx有可能是空列表的情況也可以比照判理。檢查matrix是否為空的小技巧
```python
if not any(matrix):
    return
```

# iterator
遇到sequence或是向後查找的題目，可以使用while加參數自己控制怎麼遍歷，或者是利用iterator的特性。  
舉個例子，如果想知道s2是不是s1的subsequence時，可以這麼做：
```python
def is_subsequence(s1, s2):
    return all(c in iter(s1) for c in s2)
```
上面的例子，假如s1 = "aabbcc"，s2 = "abc"，當c=a時，第一個 c in iter(s1)，c="a"會停在idx=0的地方，第二個 c in iter(s1)， c="b"
從idx=1的位置接續查找，並且停在idx=2的位置，以此類推，最後就能得到我們想要的結果。

這個技巧算是前面的 next 用法進階版。

# 兩數字是否同正負號
a ^ b > 0 

# flatten list
想要攤平巢狀 list 可以使用以下工具  
```python
list comprehension: A = [ele for sub in arr for ele in sub]
```
```python
itertools: A = list(itertools.chain.from_iterable(arr))
```
```python
reduce: A = functools.reduce(operator.iconcat, arr, [])
```

# 好用的builtin func
python提供許多好用的builtin func，例如operator.itemgetter可以取代 labmda x: x[1]

```python
from operator import itemgetter
sorted(arr, key=itemgetter(1))
# 等價於 sorted(arr, key=lambda x: x[1])
```
# 反向遍歷
因為 python 的 list 提供 negative indexing， 如果要反向遍歷時，可以用 ~i 來獲得對應於 i 的反向 indexing，像是：

```python
for i in range(len(arr)):
    arr[i] += xxx  # arr[0],  arr[1],  arr[2] , ...
    arr[~i] += ooo # arr[-1], A[-2], arr[-3], ...
```

# zip
zip 函數的作用是將 iterables 的元素打包配對，例如
```python
a = "abcd"
b = (1, 2, 3, 4, 5)
c = [6, 7, 8, 9, 0]
d = zip(a, b, c)

# d: [('a', 1, 6), ('b', 2, 7), ('c', 3, 8), ('d', 4, 9), (None, 5, 0)]
# 元素的長度可以不一樣，空值會zip會補None
```

在matrix的問題裡，用zip會有奇效，例如，旋轉矩陣
```python
/*
 * clockwise rotate
 * first reverse up to down, then swap the symmetry
 * 1 2 3     7 8 9     7 4 1
 * 4 5 6  => 4 5 6  => 8 5 2
 * 7 8 9     1 2 3     9 6 3
*/
```
正常寫法
```
def rotate(self, matrix):
    n = len(matrix)
    matrix.reverse()
    for i in xrange(n):
        for j in xrange(i, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
```
但如果用 zip 的話，只需要一行code
```python
def rotate(self, matrix):
    return matrix[:] = zip(*matrix[::-1])
```

