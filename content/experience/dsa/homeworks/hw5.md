---
title: "HW5"
date: Homework
mainSectionTitle: "ptc"
---
[Download problem set](https://dk3156.github.io/experience/dsa/homeworks/dsa_hw5.pdf)
## Answers

## Q1
```
from ArrayStack import *
def postfix_translator(tokens,variables): 
    numbers = ArrayStack() 
    ops = "+-/*"
    for i in tokens: 
        if i in ops: 
            num2 = numbers.pop()
            num1 = numbers.pop()
            result = None
            if(i == "+"): 
                result = num1+num2
            elif(i == "-"):
                result = num1-num2
            elif(i == "*"):
                result = num1*num2
            else: 
                result = num1/num2 
            numbers.push(result)
        else:
            if(i in variables):
                current_num = variables[i]
            else:
                current_num = int(i)
            numbers.push(current_num)
    return numbers.pop()  

expression  =  input("-->")
variables = {}
while(not(expression ==  "done()")): 
    tokens = expression.split()
    if(len(tokens) > 1 and tokens[1] == "="):
        value = postfix_translator(tokens[2:],variables)
        variables[tokens[0]] =  value
        print(tokens[0])
    else:
        value = postfix_translator(tokens, variables)
        print(value)
    expression  =  input("-->")
```
## Q2
```
from ArrayStack import * 
class MaxStack(): 
    def __init__(self): 
        self.data = ArrayStack() 
        self.maxData = None
    def is_empty(self): 
        return self.data.is_empty() 
    def __len__(self): 
        return len(self.data)
    def top(self): 
        if (self.is_empty()):
            raise Exception("Stack is empty")
        return self.data.top()[0]
    def pop(self): 
        if (self.is_empty()):
            raise Exception("Stack is empty")
        val = self.data.pop() 
        if(len(self.data) == 0): 
            self.maxData = None
        elif(not(self.maxData == self.data.top()[1])): 
            self.maxData = self.data.top()[1]
        return val[0] 
    def push(self, val): 
        if(self.maxData == None or self.data.top()[1] < val): 
            self.maxData = val
        self.data.push((val,self.maxData))
    
    def max(self): 
        if (self.is_empty()):
            raise Exception("Stack is empty")
        return self.data.top()[1]  
    
# maxS = MaxStack()
# maxS.push(3)
# maxS.push(1)
# maxS.push(6)
# maxS.push(4)
# print( maxS.max())

# print(maxS.pop())
# print(maxS.pop())

# print(maxS.max())
```

## Q3
```
from ArrayDeque import *
from ArrayStack import * 
class MidStack(): 
    def __init__(self): 
        self.data = ArrayStack() 
        self.mid = ArrayDeque()
    def is_empty(self): 
        return self.data.is_empty()

    def __len__(self): 
        return len(self.data)+len(self.mid)

    def top(self): 
        if (self.is_empty()):
            raise Exception("Stack is empty")
        return self.data.top()[0]

    def pop(self): 
        if (self.is_empty()):
            raise Exception("Stack is empty")
        index1 = self.data.top()[1]
        index2 = self.mid.last()[1] if not self.mid.is_empty() else None
        # print(index1,index2)
        if(index2 == index1): 
            return self.mid.dequeue_last()[0]
        else: 
            return self.data.pop()[0]

    def push(self, val): 
        index = len(self)
        self.data.push((val,index))

    
    def mid_push(self,val): 
        index = len(self)//2-1
        if(len(self)%2 == 1):
            index += 1 
        self.mid.enqueue_first((val,index))

    
# midS = MidStack()
# midS.push(2)
# midS.push(4)
# midS.push(6)
# midS.push(8)
# # midS.push(10)
# midS.mid_push(10)
# print(midS.pop())

# print(midS.pop())

# print(midS.pop())

# print(midS.pop())

# print(midS.pop())
```

## Q4
```
from ArrayStack import *
class Queue: 
    def __init__(self):
          self.queue = ArrayStack() 
          self.stack = ArrayStack()
    def __len__(self):
        return len(self.queue)+len(self.stack)
    def is_empty(self):
        return (len(self.queue)+len(self.stack) == 0 )

    def enqueue(self, elem):
        self.stack.push(elem)

    def dequeue(self):
        if(self.queue.is_empty()): 
            while(not self.stack.is_empty()): 
                self.queue.push(self.stack.pop())
        return self.queue.pop()           
    def first(self):
        if(self.queue.is_empty()): 
            while(not self.stack.is_empty()): 
                self.queue.push(self.stack.pop())
        return self.queue.top()
```

## Q5
```
from ArrayStack import * 
from ArrayQueue import * 
def permutations(lst): 

    s = ArrayStack() 
    q = ArrayQueue() 
    # print("hello")
    for i in range(len(lst)): 
        q.enqueue(([lst[i]], lst[:i]+lst[i+1:]))
    res = []
    while(not q.is_empty()): 
        # print(current)
        current = q.dequeue() 
        cLst = current[1]
        if(len(cLst) == 0): 
            res.append(current[0])
            continue
        for i in range(len(cLst)): 
            q.enqueue(( current[0]+[cLst[i]], cLst[:i]+cLst[i+1:] ) )
    return res 

    
# lst = [1,2,3,4]
# print(len(permutations(lst)))
```