---
title: "HW2"
date: Homework
mainSectionTitle: "ptc"
---
[Download problem set](https://dk3156.github.io/experience/dsa/homeworks/dsa_hw2.pdf)
## Answers

## Q3
```
import math

def factors(num):
    for i in range(1, int(math.sqrt(num))+1):
        if num % i == 0:
            yield i
    for i in range(int(math.sqrt(num))-1, 0, -1):
        if num % i == 0:
            yield num // i

def main():
    for curr_factor in factors(100):
        print(curr_factor, end=' ')

#main()
```
## Q4
```
def e_approx(n):
    res = 1
    product = 1
    for i in range(1, n+1):
        product *= i
        res += 1 / product
    return res

def main():
    for n in range(15):
        curr_approx = e_approx(n)
        approx_str = "{:.15f}".format(curr_approx)
        print("n =", n, "Approximation:", approx_str)

#main()

```

## Q5
```
def split_parity(lst):
    odd = 0
    for i in range(len(lst)):
        if lst[i] % 2 != 0: #odd
            lst[odd], lst[i] = lst[i], lst[odd]
            odd += 1

def main():
    lst = [1,2,3,4,5,6]
    split_parity(lst)
    print(lst)

#main()
```

## Q6
```
def two_sum(srt_lst, target):
    left = 0
    right = len(srt_lst)-1
    while left < right:
        if srt_lst[left] + srt_lst[right] == target:
            return (left,right)
        elif srt_lst[left] + srt_lst[right] > target:
            right -= 1
        else:
            left += 1
    return None

def main():
    srt_lst = [-2, 7, 11, 15, 20, 21]
    print(two_sum(srt_lst, 22))

#main()
```

## Q6
```
def fibs(n):
    first = 0
    second = 1
    yield 1
    for i in range(n-1):
        result = first + second
        first = second
        second = result
        yield result

def main():
    for curr in fibs(8):
        print(curr, end=" ")

#main()
```

## Q7
```
def findChange(lst01):
    left = 0
    right = len(lst01)-1
    mid = right // 2
    while True:
        if left == (right-1):
            return right
        elif lst01[mid] == 0:
            left = mid
            mid = (left + right) //2
        else:
            right = mid
            mid = (left + right) //2

def main():
    lst = [0,1]
    print(findChange(lst))

#main()
```