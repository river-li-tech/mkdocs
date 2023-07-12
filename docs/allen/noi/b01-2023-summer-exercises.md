---
comments: true
tags:
  - 信息学
---

### 2023.7.13 （90 min）
* 练习题：
    * [E05	我家的门牌号](http://hihocoder.openjudge.cn/2021summers3/E05/) 
    * [E06	不定方程求解](http://hihocoder.openjudge.cn/2021summers3/E06/) 
    * [E09	数字方格](http://hihocoder.openjudge.cn/2021summers3/E09/) 
    * [E10	满足条件的整数](http://hihocoder.openjudge.cn/2021summers3/E10/) 

* 整理笔记：
    * 每道题写出推理过程，将 **关键思路** 整理到笔记中；

### 2023.7.12 （90 min）
* 练习题：
    * [S04	修改数组](http://hihocoder.openjudge.cn/2021summers3/S04/)
    * [S05	领地选择](http://hihocoder.openjudge.cn/2021summers3/S05/)

* 整理笔记：
    * 试着阅读S04.cpp和S05.cpp代码
    * 每道题写出推理过程，将 **关键思路** 整理到笔记中；

### 2023.7.11 （90 min）
* 练习题：
    * [S03	最大和](http://hihocoder.openjudge.cn/2021summers3/S03/)
    * [S04	修改数组](http://hihocoder.openjudge.cn/2021summers3/S04/)
    * [S05	领地选择](http://hihocoder.openjudge.cn/2021summers3/S05/)

* 整理笔记：
    * 试着阅读S03-2.cpp代码
    * 每道题写出推理过程，将 **关键思路** 整理到笔记中；

### 2023.7.10 （90 min）
* 练习题：
    * [S01	前缀和](http://hihocoder.openjudge.cn/2021summers3/S01/)
    * [S02	部分和](http://hihocoder.openjudge.cn/2021summers3/S02/)
    * [S03	最大和](http://hihocoder.openjudge.cn/2021summers3/S03/)
  
* 整理笔记：
    * 在笔记中新建专题 **前缀和** 
    * 每道题写出推理过程，将 **关键思路** 整理到笔记中；
    * 对比以上3道题，思考相互之间的 **区别和联系**
    * 在笔记中写出 **前缀和** 算法的代码模板，用红笔标记关键的位置，并写下自己的理解

* 提示  
    * 前缀和 数学描述：  
        假设有数组  a[0], a[1], a[2] ...... a[n]
        前缀和  
            sum[0] = 0   
            sum[1] = a[0]  
            ...  
            sum[i] = a[0] + .... + a[i - 1]  
            即 **sum[i] = sum[i - 1] + a[i - 1]**
    * 区间和： 
        区间[i, j]的和   **rang[i, j] = sum[j + 1] - sum[i]**

    * 前缀和 举例：   
        数组序号    0       1       2       3       4       5       6    ...    n  
        数组举例    4       -3      6       3       -4      9  
        前缀和      0       4       1       7       10      6       15  
>> 能在限定时间内完成习题并做完笔记，效率很高！！


### 2023.7.7 （90 min）
* 练习题：
    * [B09	矩形分割](http://hihocoder.openjudge.cn/2021summers3/B09/)
    * [B11	矩形分割数据加强版](http://hihocoder.openjudge.cn/2021summers3/B11/)
> 提示：B11和B09题意完全相同，只是B11比B09数据更严格，二次查找下标时如果遍历[minp, r]仍可能超时，
> 所以需要用2次二分查找

* 整理笔记：
    * 每道题写出推理过程，将 **关键思路** 整理到笔记中；

> 完成得非常好，经过自己的独立思考，能将问题解决，是学习编程最好的方式

### 2023.7.6 （90 min）
* 练习题：
    * [B09	矩形分割](http://hihocoder.openjudge.cn/2021summers3/B09/)
  
* 整理笔记：
    * 每道题写出推理过程，将 **关键思路** 整理到笔记中；
  
### 2023.7.5 （90 min）
* 练习题：
    * [B07 查找学生姓名](http://hihocoder.openjudge.cn/2021summers3/B07/)
    * [B13 小朋友分巧克力](http://hihocoder.openjudge.cn/2021summers3/B13/)
  
* 整理笔记：
    * 每道题写出推理过程，将 **关键思路** 整理到笔记中；

* 尝试阅读以下文章：
    * [C++ lower_bound自定义比较详解](https://zhuanlan.zhihu.com/p/627464912)
    * 运行文章中的的示例代码
    * 将文章的重点内容整理到笔记中
    * 
### 2023.7.4 （100 min）
* 练习题：
    * ✔ [B04	在不下降数组中查找X](http://hihocoder.openjudge.cn/2021summers3/B04/) ★
    * ✔ [B08	查找最接近的元素](http://hihocoder.openjudge.cn/2021summers3/B08/) ★
    * ✔ [G:查找小于X的最大值](http://hihocoder.openjudge.cn/2023springs3xdafterexam/G/) ★★
    * ✔ [B12	加工产品](http://hihocoder.openjudge.cn/2021summers3/B12/) ★★★

* 整理笔记：
    * ✔ 新建笔记专题：**二分查找** 算法；
    * 在笔记中写出 **二分查找算法** 基础模板；
    * 通过完成以上4道练习题，将你从中发现的 **关键技巧** 整理到笔记中；

* 尝试阅读以下文章：
    * [C++ lower_bound自定义比较详解](https://zhuanlan.zhihu.com/p/627464912)
    * 运行文章中的的示例代码
    * 将文章的重点内容整理到笔记中
  
> 4道练习题完成得很好，能独立调试代码，很有进步；笔记做得很用心，继续加油 ✌
  
