---
title: "DSA lab 7"
date: Labs
mainSectionTitle: "ptc"
---

Problem set is not available.
## Answer for Coding Questions
```
def find_min(lst,low,high): 
    if(low == high): 
        return lst[low]
    return min(find_min(lst,low+1,high), lst[low])

def find_second_min(lst): 
    return find_second_min_helper(lst,0,len(lst)-1)[1]


def find_second_min_helper(lst,low,high): 
    if(low == high): 
        return [lst[low],None]
    res = find_second_min_helper(lst,low+1,high)
    if(lst[low] < res[0]): 
        holder = res[0]
        res[0] = lst[low]
        res[1] = holder 
    elif(res[1] == None or lst[low] < res[1]): 
        res[1] = lst[low]
    return res

def quick_select(lst, low, high, k):
    if(len(lst) == 0):
        return None 
    pivot = lst[-1]
    # print(pivot)
    left = [i for i in lst if(i < pivot)]
    right = [i for i in lst if(i > pivot)]
    if(low+len(left)+1 == k): 
        return pivot 
    else: 
        resL = quick_select(left, low, high-len(right)+1,k)
        resR = quick_select(right, low+len(left)+1, high, k)
        if(resL == resR): 
            return None
        else: 
            if(resL != None): 
                return resL 
            return resR 
    
    # print(left, right)

print("hello", find_second_min( [13, 9, 16, 3, 4, 2]))
lst = [13, 9, 16, 3, 4, 2] 
# print(lst[-1])
print(quick_select(lst, 0,len(lst)-1,5))

def nested_depth_level(lst): 
    depth = 1
    for i in lst: 
        if isinstance(i, list):
            current = nested_depth_level(i)+1
            depth = max(current, depth) 
    return depth

lst = [ [1, 2], 3, [4, [5, 6, [7], 8 ] ], [ [ [ [9] ] ] ] ] 

print(nested_depth_level([ [1, 2], 3, [4, [5, 6, [7], 8 ] ] ]))

def deep_reverse(lst): 
    if(not isinstance(lst, list)): 
        return 
    for i in range(len(lst)//2): 
        holder = lst[i]
        lst[i] = lst[len(lst)-1-i]
        lst[len(lst)-1-i] = holder 
        deep_reverse(holder)
        deep_reverse(lst[i])
    if(len(lst) % 2 == 1): 
        deep_reverse(lst[len(lst)//2])

deep_reverse(lst)
print(lst)
```