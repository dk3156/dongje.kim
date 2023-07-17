---
title: "HW4"
date: Homework
mainSectionTitle: "ptc"
---
[Download problem set](https://dk3156.github.io/experience/dsa/homeworks/dsa_hw4.pdf)
## Answers

## Q3
```
def print_triangle(n): 
    if(n <= 1): 
        print("*")
        return
    print_triangle(n-1)
    print("*"*n)

def print_oposite_triangles(n): 
    if(n <= 0): 
        return 
    print("*"*n)
    print_oposite_triangles(n-1)
    print("*"*n)

def print_ruler(n):
    if(n <= 1): 
        print("-")
        return
    print_ruler(n-1)
    print("- "*n)
    print_ruler(n-1)

# print_triangle(4)
# print_oposite_triangles(4)
# print_ruler(4)
```
## Q4
```
def list_min(lst, low, high): 
    if(low == high): 
        return lst[low]
    prev = list_min(lst,low+1,high) 
    return min(lst[low], prev)

# l = [2,2,3,1,5,6,7,8]
# print(list_min(l, 0, len(l)-1))
```

## Q5
```
def count_lowercase(s, low, high):
    if(low == high):
        return 1 if s[low].islower() else 0 
    res = count_lowercase(s,low+1,high)
    res += 1 if s[low].islower() else 0 
    return res 


def is_number_of_lowercase_even(s, low, high):
    count = count_lowercase(s,low,high)
    return (count % 2 == 0)
```

## Q6
```
def appearances(s, low, high):
    if(low > high): 
        return {} 
    res = appearances(s,low+1,high)
    if(s[low] in res): 
        res[s[low]] += 1 
    else: 
        res[s[low]] = 1
    return res
# print(appearances("Hello world", 0, 10))s
```

## Q7
```
def split_by_sign(lst, low, high):
    if(low == high): 
        return 
    split_by_sign(lst,low+1,high)
    if(lst[low] > 0): 
        holder = lst[low]
        counter = low
        while(counter+1 <= high and lst[counter+1] < 0): 
            lst[counter] = lst[counter+1]
            lst[counter+1] = holder
            counter += 1
    
# lst = [-1,1,-1,-1,1,-1,1,-1,1,1,-1]
# split_by_sign(lst, 0, len(lst)-1)
# print(lst)
```

## Q8
```
def flat_list(nested_lst, low, high):
    if(low > high):
        return [] 
    res = flat_list(nested_lst,low, high-1)
    if isinstance(nested_lst[high], list): 
        add = flat_list(nested_lst[high], 0, len(nested_lst[high])-1)
    else: 
        add = [nested_lst[high]]
    res =  res + add
    return res 

# res = flat_list([[1, 2], 3, [4, [5, 6, [7], 8]]],0, 2)
# print(res)
```