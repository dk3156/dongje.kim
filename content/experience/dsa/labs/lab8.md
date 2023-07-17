---
title: "DSA lab 8"
date: Labs
mainSectionTitle: "ptc"
---

[Download problem set](https://dk3156.github.io/experience/dsa/labs/dsa_lab8.docx)
## Answer for Coding Questions
```
from stack import * 
def stack_sum(s): 
    res = 0
    holder = ArrayStack() 
    while(not s.is_empty()): 
        val = s.pop() 
        res += val 
        holder.push(val)
    while(not holder.is_empty()): 
        s.push(holder.pop())
    return res 


def eval_prefix(exp_str):
    """
    : exp type: str
    : return type: int
    """
    exp_lst = exp_str.split()
    s = ArrayStack()
    exp = "+*/-"
    print('hi')
    for i in range(len(exp_lst)-1,-1,-1): 
        if exp_lst[i] in exp: 
            num1 = int(s.pop())
            num2 = int(s.pop())
            if(exp_lst[i] == "*"): 
                s.push(num1*num2)
            elif(exp_lst[i] == "+"): 
                s.push(num1+num2)
            elif(exp_lst[i] == "/"): 
                s.push(num1/num2)
            else: 
                s.push(num1-num2)

        else: 
            s.push(exp_lst[i])
    return s.top()

st = " - + * 16 5 * 8 4 20" 
print(eval_prefix(st))

def flatten_list(lst):
    """
    : lst type: list
    : return type: None
    """
    s = ArrayStack()
    for i in reversed(lst): 
        s.push(i)
    lst.clear() 
    while(not s.is_empty()): 
        current = s.pop()
        if(isinstance(current, list)): 
            for i in reversed(current): 
                s.push(i)
        else: 
            lst.append(current)
        
lst = [ [[[0]]], [1, 2], 3, [4, [5, 6, [7]], 8], 9]
flatten_list(lst)
print(lst)
```