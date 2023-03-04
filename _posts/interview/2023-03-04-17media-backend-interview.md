---
layout: post
title:  "[Interview] 17 Media Backend"
author: tony
categories: [interview, backend]
image: 17Media-backend-interview.png
tags: [interview,backend,17media]
---
以下是網路上找到的 17 Media 後端面試題目，並且加上個人的解法。

# 面試流程
1. Codility
2. On Site 白板題
3. Behavior Question
4. chat with CEO

# Questions
## leetcode類型
### Q1. 
對 string 格式儲存的數字做減法，e.g. s1 "1234", s2 "234"，回傳 "1000"。  
### ANS. 
考慮到s1, s2有可能是一串非常長的字串，轉換回數字有可能超出int的上限，因此不能直接用字串轉數字再轉回字串的方式，必須要用digit operation再append到結果。   
pseudo code如下
```python
def sub(s1:tr, s2:str) -> str:
   if len(s1) < len(s2):
      return "-" + sub(s2, s1)
   # 紀錄是否需要借位
   carry=0
   res=""
   # 從最右邊開始
   for i in range(len(s2)-1,-1,-1):
    # 反向遍歷 
        digit = int(s1[i]) - int(s2[i]) - carry
        carry = 0
        if digit<0:
            digit+=10
            carry=1
        res=str(digit)+res
    # post process
    if len(s1)==len(s2) and carry == 1:
        return "-"+ res
    if len(s1) > len(s2): 
        return sub(s1[:len(s1)-len(s2)], str(carry))+res
    if res[0]=="0":
        return res[1:]
```

### Q2. 
pair number 取交集
### ANS.
略
### Q3.
氣泡排序法
### ANS.
略
### Q4.
數列中有個數字的次數出現了一半以上，請找出該數字
### ANS.
Time: O(n) Space: O(1)的解法，用一個變數cnt儲存數字出現的次數，另一個變數res儲存候選人，從頭開始遍歷數列，
假如遍歷到的數字和候選人一樣，則cnt加一，如果兩者不同則cnt減一，當cnt為零時，將候選人改為當前的數字，且初始化cnt為1，最後留下來的候選人即為所求。
### Q5.
給一個不重複數列與數字K，找出三個數字使此三數總和小於K
### ANS.
先對數列排序，再來用雙指標從頭尾開始遍歷，時間複雜度是O(n*log(n))
### Q6.
給一個字串的陣列，找出出現在所有字串的共同的最長字首
### ANS.
暴力法，map reduce整個陣列，兩兩比對找出共同字首，遍歷完後就能得到最長共同字首。
### Q7.
給一個已排序數列，找出其中某數字的開始(頭)跟結束(尾)的index
### ANS.
Binary Search，做兩次binary search，第一次找頭，第二次找尾，時間複雜度是O(log(n))

## distributed system
### Q1. 
如何在多個 DB 間打 transaction，假設跨 DB 的款項轉移
### ANS.
Distributed Transaction  
https://rickhw.github.io/2020/05/16/DistributedSystems/Distributed-Transactions/  
分佈式交易涉及多個 DB 時，無法在同一個Transaction裡頭進行原子性操作，只能透過以下的方式保證最終一致性。
- 2PC
- 3PC
- Message Queue
## Database
### Q1. 
如何在 DB 紀錄樹狀結構，快速取出子樹   
### ANS.
參考答案如下，最佳解是 NESTED SET MODEL，另外最常見用來在DB儲存樹狀結構的方式是self reference model，再透過multi query根據樹高拿資料，
很明顯後者不是面試官要的答案。
   1. self join 
   2. CTE wtih語法(遞迴)
   3. store procedure或function
   4. 透過程式做遞迴查詢
   5. 反正規劃設計資料庫,平面化資料
   6. nested set model  
   https://columns.chicken-house.net/2019/06/01/nested-query/  
   https://www.sitepoint.com/hierarchical-data-database-2/  
   (推薦) 飛鳥實驗室-用 Nested Set Model 建立巢狀資料表 http://asika.windspeaker.co/post/3488-nested-set-model

## System Design
### Q1.
簡單設計一個通話軟體
