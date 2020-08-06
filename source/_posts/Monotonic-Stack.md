---
title: Monotonic Stack
date: 2020-08-05 16:43:52
tags:
- Data Structure
- Algorithm
categories:
- Data Structure
- Algorithm
---

持续施工中

### **What is mono stack?**

Monotonic stack is actually a stack. It just uses some ingenious logic **to keep the elements in the stack orderly** (monotone increasing or monotone decreasing) after each new element putting into the stack.

本质上来说，单调栈就是一个栈。只不过我们在元素加入过程中会进行pop操作来保证栈的单调性。

### **Advantage** of mono stack:

**Linear time complexity**, all elements will only be **put into the stack once**, and once they are out of the stack, they will not come in again.

使用单调栈往往意味着线性复杂度解决问题，所以如果你发现了题目有着单调栈的pattern，那么就可以尝试使用单调栈优化算法。

### **Properties of monotonic stack**

1. The elements in the monotonic stack are monotonic

2. Before elements are added to the stack, all elements that **destroy the monotonicity of the stack** will be **deleted** at the top of the stack

### **What kind of problem use mono stack?**

The problems that **cares about** the first element **smaller/greater** than it in the **left/right**.

很关键的一个问题，什么样的问题能够使用单调栈呢？

其实我们会发现，单调栈只擅长解决很小的一类问题，它的pattern十分明显：

那就是如果问题关心**第一个左边/右边比它小/大**的元素，那么就可以使用单调栈。

### **Intuition behind mono stack**

Suppose we want to solve this question: 

Given an array, we want to output the **first** element **smaller** than it in the **left** for every number in that array.

What is the **naive** way? 

Just use two for/while loop to solve it brute forcely.

But clearly there are many repeating computation. So how to solve it efficiently?

We store elements before ai into a stack:[a0~ai-1] .

And each time search through this stack.

**But what if there are a pair aj >= ak(j < k):****

Then aj will never be chosen**.**

**Thus for all j < k, aj < ak. AKA **monotonic**.

好了，说了半天，我们来举一个例子吧。

考虑这样一个问题：

给定一个数组，对于该数组中的每个数字，我们要输出的左边第一个比它小的数字。

什么是“天真”的方式？

只需使用两个for / while循环即可强行解决它。

但是显然有很多重复的计算。 那么如何有效地解决呢？

就是使用单调栈

### **Code template**

```python
stk = []
for i in range(len(nums)):
    while (stk && check(stk[-1], nums[i])): stk.pop()
    stk.append(nums[i])
def check(num1, num2):
	return num1 >= num2
```

### **Extension**

We just solved smaller/left.

How about greater or right?

for i in range(len(nums) - 1, -1, -1):

return num1 <= num2

我们可以轻松地扩展单调栈应用到比右边或者第一个比它大的数。

### **Problems**

496 Next Greater Element I: https://leetcode.com/problems/next-greater-element-i/

503 Next Greater Element II: https://leetcode.com/problems/next-greater-element-ii/. 

42 Trapping Rain Water: https://leetcode.com/problems/trapping-rain-water/. 

84 Largest Rectangle in Histogram: https://leetcode.com/problems/largest-rectangle-in-histogram/

### **Reference**

1. [https://github.com/labuladong/fucking-algorithm/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E7%B3%BB%E5%88%97/%E5%8D%95%E8%B0%83%E6%A0%88.md](https://github.com/labuladong/fucking-algorithm/blob/master/数据结构系列/单调栈.md)

2. [单调栈的介绍以及一些基本性质_多反思，多回顾，要坚持。](https://blog.csdn.net/liujian20150808/article/details/50752861)

3. https://blog.csdn.net/qq_17550379/article/details/86519771