---
title: 八大排序算法的 Python 实现
date: 2018-03-14 13:48:20
categories:
- 笔记
tags:
- Python3
---

<!-- toc -->

## 插入排序

```python
def insert_sort(lists):
    count = len(lists)
    for i in range(1, count):
        key = lists[i]
        j = i - 1
        while j >= 0:
            if lists[j] > key:
                lists[j + 1] = lists[j]
                lists[j] = key
            j -= 1
    return lists
```

<!-- more -->

![insert_sort](/images/Insertion_sort_animation.gif)

## 希尔排序

```python
def shell_sort(lists):
    count = len(lists)
    step = 2
    group = count // step
    while group > 0:
        for i in range(0, group):
            j = i + group
            while j < count:
                k = j - group
                key = lists[j]
                while k >= 0:
                    if lists[k] > key:
                        lists[k + group] = lists[k]
                        lists[k] = key
                    k -= group
                j += group
        group //= step
    return lists
```

![shell_sort](/images/Sorting_shellsort_anim.gif)

## 冒泡排序

```python
def bubble_sort(lists):
    count = len(lists)
    for i in range(0, count):
        for j in range(i + 1, count):
            if lists[i] > lists[j]:
                lists[i], lists[j] = lists[j], lists[i]
    return lists
```

![bubble_sort](/images/Bubble_sort_animation.gif)

## 快速排序

```python
qs = lambda xs: ((len(xs) <= 1 and [xs]) or [qs([x for x in xs[1:] if x < xs[
                 0]]) + [xs[0]] + qs([x for x in xs[1:] if x >= xs[0]])])[0]


def quick_sort(lists, left=0, right=9):
    if left >= right:
        return lists
    key = lists[left]
    low = left
    high = right
    while left < right:
        while left < right and lists[right] >= key:
            right -= 1
        lists[left] = lists[right]
        while left < right and lists[left] <= key:
            left += 1
        lists[right] = lists[left]
    lists[right] = key
    quick_sort(lists, low, left - 1)
    quick_sort(lists, left + 1, high)
    return lists
```

![quick_sort](/images/Sorting_quicksort_anim.gif)

## 选择排序

```python
def select_sort(lists):
    count = len(lists)
    for i in range(0, count):
        min = i
        for j in range(i + 1, count):
            if lists[min] > lists[j]:
                min = j
        lists[min], lists[i] = lists[i], lists[min]
    return lists
```

![select_sort](/images/Selection_sort_animation.gif)

## 堆排序

```python
def adjust_heap(lists, i, size):
    lchild = 2 * i + 1
    rchild = 2 * i + 2
    max = i
    if i < size // 2:
        if lchild < size and lists[lchild] > lists[max]:
            max = lchild
        if rchild < size and lists[rchild] > lists[max]:
            max = rchild
        if max != i:
            lists[max], lists[i] = lists[i], lists[max]
            adjust_heap(lists, max, size)


def build_heap(lists, size):
    for i in range(0, (size // 2))[::-1]:
        adjust_heap(lists, i, size)


def heap_sort(lists):
    size = len(lists)
    build_heap(lists, size)
    for i in range(0, size)[::-1]:
        lists[0], lists[i] = lists[i], lists[0]
        adjust_heap(lists, 0, i)
    return lists
```

![heap_sort](/images/Sorting_heapsort_anim.gif)

## 归并排序

```python
def merge(left, right):
    i, j = 0, 0
    result = []
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result += left[i:]
    result += right[j:]
    return result


def merge_sort(lists):
    if len(lists) <= 1:
        return lists
    num = len(lists) // 2
    left = merge_sort(lists[:num])
    right = merge_sort(lists[num:])
    return merge(left, right)
```

![merge_sort](/images/Merge_sort_animation2.gif)

## 基数排序

```python
import math


def radix_sort(lists, radix=10):
    k = int(math.ceil(math.log(max(lists), radix)))
    bucket = [[] for _ in range(radix)]
    for i in range(1, k + 1):
        for j in lists:
            bucket[j // (radix**(i - 1)) % radix].append(j)
        del lists[:]
        for z in bucket:
            lists += z
            del z[:]
    return lists
```

![merge_sort](/images/radix_sort.gif)

## 测试

```python
import random
original_test = list(random.randint(1, 100) for _ in range(10))
print("原始列表：    %s" % original_test)

# 插入排序
insert_test = insert_sort(original_test)

# 希尔排序
shell_test = shell_sort(original_test)

# 冒泡排序
bubble_test = bubble_sort(original_test)

快速排序
quick_test = quick_sort(original_test)

# 直接选择排序
select_test = select_sort(original_test)

# 堆排序
heap_test = heap_sort(original_test)

# 归并排序
merge_test = merge_sort(original_test)

# 基数排序
radix_test = radix_sort(original_test)

print("插入排序：    %s" % insert_test)
print("希尔排序：    %s" % shell_test)
print("冒泡排序：    %s" % bubble_test)
print("快速排序：    %s" % quick_test)
print("直接选择排序：%s" % select_test)
print("堆排序：      %s" % heap_test)
print("归并排序：    %s" % merge_test)
print("基数排序：    %s" % radix_test)
print("快速排序：    %s" % qs(original_test))
```

本文来自：[八大排序算法的 Python 实现](http://python.jobbole.com/82270)
