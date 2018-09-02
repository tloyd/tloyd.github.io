---
layout: post
title:  "用Python计算一道特别的数独"
date:   2018-08-31 14:49:12
categories: 代码共享
tags: Python
author: 何帆
---

* content
{:toc}

> GF发来了一道特别的数独题目,除了数独原有的规则外,这道题里还有四块区域,这四块区域里的数字,也不能重复.






##题目

![骨灰级数独][1]

## 基本方法

计算机编程,总体来说,就是一种方法论.而数独这个游戏，关键在这个“独”字上，横竖不能重复，方块不能重复。在这道题里,所有的灰色方块也不能重复,因此,又多了一个限制条件.

大致思路:从第一个空格开始试着填数，从 1 开始判断是否符合规则，如果 1 不满足横排竖排九宫格以及灰色区域无重复的话，就再判断2 ，以此类推，直到填入一个暂时满足规则的数，中断此格，移动到下一个空格重复这个过程。

如果到达某个空格发现已经无数可选了，说明前面某一格填错了，那就返回上一格并且置零，从上一格的中断数字继续往 9 尝试，直到这样回朔到填错的那一格。

这样的话，我们就可以整理出重要的步骤了

 - 寻找空格
 - 从数字 1 到 9中取数,并循环判断是否符合规则
 - 填入数字

## 代码实现

```
# encoding: utf-8
# @author: HeFan & YangJing
# @contact: hftloyd@gmail.com
# @file: Main.py
# @time: 2018/8/30 11:29
# @desc:

# 打印结果
def print_grid(arr):
    for i in range(9):
        print("----------------------------------------------")
        for j in range(9):
            print("|",arr[i][j]," ", end="")
        print("|")
    print("-----------------------------------------------")


# 找出空格
def find_empty_location(arr, l):
    for row in range(9):
        for col in range(9):
            if arr[row][col] == 0:
                l[0] = row
                l[1] = col
                return True
    return False


# 找出num在该行是否重复
def used_in_row(arr, row, num):
    for i in range(9):
        if arr[row][i] == num:
            return True
    return False


# 找出num在该列是否重复
def used_in_col(arr, col, num):
    for i in range(9):
        if arr[i][col] == num:
            return True
    return False


# 找出num在该方块的3X3区域内是否重复
def used_in_box(arr, row, col, num):
    for i in range(3):
        for j in range(3):
            if arr[row+i][col+j] == num:
                return True
    return False

# 找出num在灰色部门是否出现过!
def used_in_greybox(arr, row, col, num):
    if row>=1 and row<=3 and col>=1 and col<=3:
        row=row-(row-1)%3
        col=col-(col-1)%3
        for i in range(3):
            for j in range(3):
                if arr[row+i][col+j] == num:
                    return True
        return False
    if row>=1 and row<=3 and col>=5 and col<=7:
        row=row-(row-1)%3
        col=col-(col-5)%3
        for i in range(3):
            for j in range(3):
                if arr[row+i][col+j] == num:
                    return True
        return False
    if row>=5 and row<=7 and col>=1 and col<=3:
        row=row-(row-5)%3
        col=col-(col-1)%3
        for i in range(3):
            for j in range(3):
                if arr[row+i][col+j] == num:
                    return True
        return False
    if row>=5 and row<=7 and col>=5 and col<=7:
        row=row-(row-5)%3
        col=col-(col-5)%3
        for i in range(3):
            for j in range(3):
                if arr[row+i][col+j] == num:
                    return True
        return False

# 检查num是否符合规则
def check_location_is_safe(arr, row, col, num):
    return not used_in_row(arr, row, num) and not used_in_col(arr, col, num) and not used_in_box(arr, row - row % 3, col - col % 3, num) and not used_in_greybox(arr, row , col , num)


def solve_sudoku(arr):
    l = [0, 0]
    if not find_empty_location(arr, l):
        return True
    # 未被填充的位置，赋值给row，col
    row = l[0]
    col = l[1]

    for num in range(1, 10):
        if check_location_is_safe(arr, row, col, num):
            arr[row][col] = num
            if solve_sudoku(arr):
                return True
            # 若当前num导致未来并没有结果，则当前所填充的数无效，置0后选下一个数测试
            arr[row][col] = 0

    return False


if __name__ == "__main__":
    import datetime
    begin = datetime.datetime.now()
    grid = [[0 for x in range(9)] for y in range(9)]

    grid = [[0, 0, 0, 8, 0, 0, 0, 0, 0],
            [0, 6, 9, 0, 0, 0, 3, 0, 0],
            [0, 0, 0, 3, 0, 0, 0, 8, 0],
            [1, 0, 0, 0, 3, 0, 0, 0, 0],
            [5, 0, 0, 9, 0, 4, 0, 0, 7],
            [0, 0, 0, 0, 1, 0, 0, 0, 3],
            [0, 4, 0, 0, 0, 9, 0, 0, 0],
            [0, 0, 3, 0, 0, 0, 6, 4, 0],
            [0, 0, 0, 0, 0, 3, 0, 0, 0]]
    if solve_sudoku(grid):
        print_grid(grid)
    else:
        print("No solution exists\n")
    end=datetime.datetime.now()
    print("\ncost time:",end-begin)
```
使用了唯一候选数法和递归试填,其实数独还有其它更优解法,但具体到这道题目,也不会提高多少速度.
 
  [1]: https://wx4.sinaimg.cn/mw690/005JzrjDgy1fuv7cjhjnnj309w0ak0ug.jpg