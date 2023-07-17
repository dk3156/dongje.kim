---
title: "DSA lab 6"
date: Labs
mainSectionTitle: "ptc"
---

Problem set is not available. 
## Answer for Coding Questions
```
def sum_to(n): 
    if(n <= 0): 
        return 0 
    sum = sum_to(n-1)
    sum += n 
    return sum 
print(sum_to(100))
def product_evens(n): 
    if(n <= 0): 
        return 1 
    product = product_evens(n-2) 
    return product*n
print(product_evens(8))
def find_max(lst,low = 0 ,high = None): 
    if(high == None): 
        high = len(lst)-1
    if(low == high): 
        return lst[low]
    max_num = max(lst[low], find_max(lst,low+1,high))
    return max_num
print(find_max([13, 9, 16, 3, 4, 2]))


def is_palindrome(input_str, low, high):
    if(low >= high): 
        return True 
    prev = is_palindrome(input_str, low+1, high-1)
    return (input_str[low] == input_str[high]) and prev 

print(is_palindrome("racecar", 0, 6))
print(is_palindrome("racecar", 1, 5))
print(is_palindrome("racecar", 1, 3))


def binary_search(lst, low, high, val):
    if(low > high): 
        return None 
    mid = (low+high)//2 
    if(lst[mid] > val):
        return binary_search(lst,low,mid-1, val)
    elif(lst[mid] < val): 
        return binary_search(lst,mid+1, high, val)
    else: 
        return mid

lst = [i for i in range(0,50)]

print(binary_search(lst,0,len(lst)-1,49))
print(binary_search(lst,0,len(lst)-1,20))
print(binary_search(lst,0,len(lst)-1,10))
print(binary_search(lst,0,len(lst)-1,31))

def vc_count(word, low, high):
    vowels = "aeiouAEIOU"
    if(low > high): 
        return (0,0)
    prev = vc_count(word,low+1,high)
    if(word[low] in vowels): 
        return (prev[0]+1, prev[1])
    return (prev[0], prev[1]+1)

word = "NYUTandonEngineering"
print(vc_count(word, 0, len(word)-1))
```