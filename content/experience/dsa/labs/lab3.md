---
title: "DSA lab 3"
date: Labs
mainSectionTitle: "ptc"
---

[Download problem set](https://dk3156.github.io/experience/dsa/labs/dsa_lab3.docx)
## Answer for Coding Questions
```
import random

#1a

#explain traversal using variables that reference indices/ random access
def reverse(lst):
    low = 0
    high = len(lst) - 1
    
    while low < high: #or, for i in range(len(lst)//2)
        lst[low], lst[high] = lst[high], lst[low] #swap
        low += 1
        high -= 1

#simplified
def reverse2(lst):
    for i in range(len(lst)//2): 
        lst[i], lst[len(lst) - 1 - i] = lst[len(lst) - 1 - i], lst[i]

'''
lst = [1, 2, 3, 4, 5, 6, 7]
lst2 = [1, 2, 3, 4, 5, 6, 7, 8]
reverse(lst)
print(lst)
reverse2(lst)
print(lst)

reverse(lst2)
print(lst2)
reverse2(lst2)
print(lst2)
'''


#1b
def reverse_list(lst, low = None, high = None):
    if low is None:
        low = 0
    if high is None:
        high = len(lst)-1

    while low < high:
        lst[low], lst[high] = lst[high], lst[low] #swap
        low += 1
        high -= 1


#2        
def move_zeroes(nums):
    last_zero = 0 #keep track of the last zero
    for i in range(len(nums)): #use i to traverse through the list, 
        if nums[i] != 0: #if lst[i] != 0, swap, then move the last zero reference up
            nums[i], nums[last_zero] = nums[last_zero], nums[i]
            last_zero += 1

    #this solution also works because if last_zero is actually not referencing a 0 and neither is nums[i], it'll just swap and move BOTH pointers up


#Consider these test cases! 
print("\nZeroes in between list:")
lst = [1, 0, 0, 13, 0, 3]
move_zeroes(lst)
print(lst)

print("\nZeroes in front of list:")
lst = [0, 0, 0, 1, 13, 3]
move_zeroes(lst)
print(lst)

print("\nZeroes in back of list:")
lst = [1, 13, 3, 0, 0, 0]
move_zeroes(lst)
print(lst)

print("\nShuffle Shuffle:")

length = random.randint(5, 15)
lst = [random.randint(0, 30)*(random.randint(0, 2)) for i in range(length)] 
random.shuffle(lst)
print("Before:", lst)
move_zeroes(lst)
print("After: ", lst)



#3
'''
ex) lst = [1, 2, 3, 4, 5, 6], shift(lst, 2) -- > [5, 6, 1, 2, 3, 4]
reverse the whole list --> [6, 5, 4, 3, 2, 1]
reverse the list from 0 to k-1 (2-1 = 1) --> [5, 6, 4, 3, 2, 1]
reverse the list from k=2 to len(lst)-1 --> [5, 6, 1, 2, 3, 4]

ANALYSIS
reverse whole list costs n 
reverse list from 0 to k-1 costs k
reverse list from k to n costs n-k
together n + k + (n-k) = 2n = O(n)
'''
def shift(lst, k):
    reverse_list(lst) #reverse the whole lst
    reverse_list(lst, 0, k-1) #reverse the lst from 0 to k
    reverse_list(lst, k, len(lst)-1) #reverse the lst from k to len(lst)-1


#4
#a.Brute force solution -->   O(n*k) #don't need to explain this one
    
def findMaxAverage(nums,k):
  max_avg = 0.0

  for i in range(len(nums) - k + 1):
    _sum = 0.0  # find sum of next k elements

    for j in range(i, i + k):
      _sum += nums[j]

    average = _sum / k  # calculate the average of selected k elements
    max_average = max(max_average, average)  # update max_average

    if len(nums) == 1:  # if there is only 1 element in nums
      return average

  return max_average
    

#b.Sliding window solution -->   O(n)

def findMaxAverage(nums, k):
    start = 0
    end = start
    curr_sum = nums[start]
    curr_max = 0
    # Accumulate the sum of the first k elements
    while (end < round(len(nums) / k) - 1):
        end += 1
        curr_sum += nums[end]
    # Set the max
    curr_max = curr_sum
    # Slide the window
    while (end < len(nums) - 1 ):
        curr_sum -= nums[start]
        start += 1
        end += 1
        curr_sum += nums[end]
        curr_max = max(curr_max, curr_sum)
    return curr_max
```