---
title: "DSA lab 4"
date: Labs
mainSectionTitle: "ptc"
---

[Download problem set](https://dk3156.github.io/experience/dsa/labs/dsa_lab4.docx)
## Answer for Coding Questions
```
#1
def is_palindrome(s): 
    left = 0
    right = len(s) - 1
    while left < right: #or while left < right
        if s[left] != s[right]: return False
        left += 1
        right -= 1 return True

#simplified
def is_palindrome2(s): 
    for i in range(len(s)//2):
        if s[i] != s[len(s)-1 - i]: #or ss[-1 -i]
            return False
    return True

#2
def reverse_vowels(input_str): 
    left = 0
    right = len(input_str) - 1 vowels = "aeiou"
    lst = list(input_str) #linear
    while left < right: 
    #can't use for loop because you don't know where vowels are placed 
    if lst[left] in vowels and lst[right] in vowels:
        lst[left], lst[right] = lst[right], lst[left] left += 1
    right -= 1
    if lst[left] not in vowels: 
        left += 1
    if lst[right] not in vowels: 
        right -= 1
    return "".join(lst) #linear

#Q3
def find_missingl(lst): 
    if lst[0] != 0:
        return 0
    for i in range(1, len(lst)):
        if lst[i] - lst[i-1] > 1: 
        #difference between 2 consecutive numbers is expected to be 1
        return lst[i] - 1 
    return len(lst)
#3b binary
'''
with lst = [0, 1, 2, 3, 4, 5, 6, 7, 8 ]
we expect i == lst[i];
however, if we remove a number, there will be a shift for example, removing 3 will cause:
lst = [0, 1, 2, 4, 5, 6, 7, 8]
now, i == lst[i] only applies for numbers up to 2, everything else is lst[i] = i + 1, ex) at i = 3, we have 4
binary search is used to detect that shift:
for example (first iteration), lst = [0, 1, 2, 4, 5, 6, 7, 8]
left = 0, lst[left] = 0
right = 7, lst[right] = 8 #expected 7 here mid = 3, lst[mid] = 4
if lst[right] - lst[mid] != right - mid -> (8-4) != (7-3)
this is False, so that means we didn't detect a shift on the right side
if lst[mid] - lst[left] != mid - left -> (4-0) != (3-0)
well, this is True, so that means the shift occured somewhere on the left
so update mid -> right = mid and we eliminated the right half our search
#notice we don't do right = mid - 1 and throwaway the mid like we did for normal binary search because we are looking for a MISSING val
this is because if we did right = mid -1, we'd get right = 2 and our search would be [0, 1, 2]
we threw away the mid in binary search because we were looking for an EXISTING val and we already checked if val == lst[mid]
anyway,
continue this until lst[right] - lst[left] == 2; the missing number would've been in between
'''
#3b
def find_missingb(lst): 
    left = 0
    right = len(lst) - 1 
    if lst[0] != 0:
        return 0
    elif lst[len(lst) - 1] == len(lst) - 1:
        return len(lst)
    while lst[right] - lst[left] != 2:
        mid = (left + right)//2
        if lst[right] - lst[mid] != right - mid:
            left = mid
        elif lst[mid] - lst[left] != mid - left:
            right = mid 
    return lst[left] + 1
#3c
def find_missingc(lst):
    total = sum(lst) #linear
    for num in range(len(lst)+1):
        total -= num 
    return -total
#3c without using sum function
def find_missingc2(lst):
    total = 0
    for num in range(len(lst)):
        total -= (num - lst[num]) 
    return len(lst) - total
#3c using the n(n+1)/2 function
def find_missingc3(lst):
    #len(lst) = n
    return len(lst)*(len(lst)+1)//2 - sum(lst)
#Q4 Part A
def find_pivot(lst): 
    left = 0
    right = len(lst) - 1 mid = left
    while left < right:
        mid = (left + right)//2
        if (lst[mid] > lst[right]): #if the right bound is smaller than the mid, this part is not sorted
            left = mid + 1
            #only increment left, because the smallest should be on the left side 
        else:
        #lst[mid] < lst[left] left bound biger than mid, this part is not sorted
            right = mid 
    return left
#Q4 Part B
def shift_binary_search(lst, target): 
    mid = find_pivot(lst) #save the pivot 
    left = 0
    right = len(lst) - 1
    #now binary search while left <= right:
    mid = (left + right) //2 
    if lst[mid] == target:
        return mid
    elif lst[mid] < target:
        left = mid + 1
    else: #lst[mid] > target
        right = mid - 1
    ßreturn None #couldn't find
#5a
import math
def jump_search_k(lst, val, k):
    if len(lst) > 0: 
        curr = 0
        prev = curr 
        jump = k
    #check if list not empty first
    #save the prev jump point
    #how much you jump, this is your k
    #jump not out of bound and lst[curr] < val
    while curr < len(lst) and lst[curr] < val: 
        prev = curr
        curr += jump
    #if jump out of range for a non perfect square size
    if curr >= len(lst):
        prev = curr - jump #jump back to prev
        curr = len(lst) - 1 #change upper bound to last index
    #linear search back at most n//k values
    while curr >= prev: 
        if lst[curr] == val:
            return curr curr -= 1
    return None #couldn't find it 
    
#5b
def jump_search(lst, val):
    if len(lst) > 0: 
    #check if list not empty first 
    curr = 0
    prev = curr #save the prev jump point
    jump = math.floor(math.sqrt(len(lst))) 
    #how much you jump, this is your k
    #int( len(lst) ** (0.5) )
    #jump not out of bound and lst[curr] < val
    while curr < len(lst) and lst[curr] < val: 
        prev = curr
        curr += jump
    #if jump out of range for a non perfect square size
    if curr >= len(lst):
        prev = curr - jump
        curr = len(lst) - 1 #change upper bound to last index
    #linear search back at most sqrt(n) values, k == sqrt(n)
    while curr >= prev: 
        if lst[curr] == val:
            return curr curr -= 1
    return None #couldn't find it
'''
lst = [-1111, -818, -646, -50, -25, -3, 0, 1, 2, 11, 33, 45, 46, 51, 58, 72, 74, 75, 99, 110, 120, 121, 345, 400, 500, 999, 1000, 1114, 1134, 10010, 500000, 999999]

#Testing k
for i in range(1, len(lst)):
# print("i:",i)
    print("TESTING VALUES IN LIST:", "k = ", i, "\n")
for val in lst:
    if jump_search_k(lst, val, i) is None:
        print(val, "FAILED - DID NOT FIND")
#just to make sure you’re not stuck in an infinite loop print("TEST k COMPLETED")
#Testing sqrt
print("\nTESTING VALUES IN LIST: k = sqrt(n) \n")

for val in lst:
    if jump_search(lst, val) is None:
        print(val, "FAILED - DID NOT FIND")
#just to make sure you’re not stuck in an infinite loop print("TEST sqrt COMPLETED")
'''
#6
def exponential_search(lst, val):
    if len(lst) > 0: #check if list is not empty if lst[0] == val: #check index 0 first
        return 0 
    else:
        i = 1 #start at 1 for the exponential search while i * 2 < len(lst) and lst[i] < val:
    i *= 2
    left = i//2
    right = i
    if lst[right] < val: #check if out of bounds (exited because i * 2 < len(lst))
        right = len(lst) - 1
    #now binary search
    while left <= right:
        mid = (left + right)//2
    if lst[mid] == val: 
        return mid
    elif lst[mid] < val: 
        left = mid + 1
    elif lst[mid] > val: 
        right = mid - 1
    return None

#testing exponential search
lst = [-1111, -818, -646, -50, -25, -3, 0, 1, 2, 11, 33, 45, 46, 51, 58, 72, 74, 75, 99, 110, 120, 121, 345, 400, 500, 999, 1000, 1114, 1134, 10010, 500000, 999999]
'''
print("TESTING VALUES IN LIST:\n")

for val in lst:
    if exponential_search(lst, val) is None:
        print(val, "FAILED - DID NOT FIND")
#just to make sure you’re not stuck in an infinite loop print("TEST COMPLETED")
'''
lst = [2,3,4,1] print(shift_binary_search(lst, 1))
```