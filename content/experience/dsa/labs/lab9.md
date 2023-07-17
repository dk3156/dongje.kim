---
title: "DSA lab 9"
date: Labs
mainSectionTitle: "ptc"
---

Problem set is not available.
## Answer for Coding Questions
```
from ArrayQueue import *

class ArrayDeque:
    INITIAL_CAPACITY = 8

    def __init__(self):
        self.data = make_array(ArrayQueue.INITIAL_CAPACITY)
        self.num_of_elems = 0
        self.front_ind = None

    def __len__(self):
        return self.num_of_elems

    def is_empty(self):
        return (self.num_of_elems == 0)

    def enqueue_last(self, elem):
        if (self.num_of_elems == len(self.data)):
            self.resize(2 * len(self.data))
        if (self.is_empty()):
            self.data[0] = elem
            self.front_ind = 0
            self.num_of_elems += 1
        else:
            back_ind = (self.front_ind + self.num_of_elems) % len(self.data)
            self.data[back_ind] = elem
            self.num_of_elems += 1
    def enqueue_first(self, elem):
        if(self.num_of_elems == len(self.data)): 
            self.resize(2*len(self.data))
        if (self.is_empty()):
            self.data[0] = elem
            self.front_ind = 0
            self.num_of_elems += 1
        else:
            self.front_ind = (self.front_ind - 1) % len(self.data)
            self.data[self.first] = elem
            self.num_of_elems += 1

    def dequeue_first(self):
        if (self.is_empty()):
            raise Exception("Queue is empty")
        value = self.data[self.front_ind]
        self.data[self.front_ind] = None
        self.front_ind = (self.front_ind + 1) % len(self.data)
        self.num_of_elems -= 1
        if(self.is_empty()):
            self.front_ind = None
        elif(self.num_of_elems < len(self.data) // 4):
            self.resize(len(self.data) // 2)
        return value
    def dequeue_last(self):
        if (self.is_empty()):
            raise Exception("Queue is empty")
        back_ind = (self.front_ind + self.num_of_elems) % len(self.data)-1
        value = self.data[back_ind]
        self.data[back_ind] = None
        self.num_of_elems -= 1
        if(self.is_empty()):
            self.front_ind = None
        elif(self.num_of_elems < len(self.data) // 4):
            self.resize(len(self.data) // 2)
        return value
    def first(self):
        if self.is_empty():
            raise Exception("Queue is empty")
        return self.data[self.front_ind]

    def resize(self, new_cap):
        old_data = self.data
        new_data = make_array(new_cap)
        old_ind = self.front_ind
        for new_ind in range(self.num_of_elems):
            new_data[new_ind] = old_data[old_ind]
            old_ind = (old_ind + 1) % len(old_data)
        self.data = new_data
        self.front_ind = 0

test = ArrayDeque() 
for i in range(20): 
    test.enqueue_last(i)

for i in range(10): 
    print(test.dequeue_first())
    print(test.dequeue_last())



class MeanQueue:
    def __init__(self):
        self.data = ArrayQueue()
        self.sum = 0 
    def __len__(self):
        '''Return the number of elements in the queue'''
        return len(self.data)
    def is_empty(self):
        ''' Return True if queue is empty'''
        return self.data.is_empty() 
    def enqueue(self, e):
        ''' Add element e to the front of the queue. If e is not
        an int or float, raise a TypeError '''
        if(not(isinstance(e, int) or isinstance(e,float))): 
            raise Exception("Input is not a number")
            return
        self.sum += e 
        self.data.enqueue(e)
    def dequeue(self):
        ''' Remove and return the first element from the queue. If
        the queue is empty, raise an exception'''
        if(self.is_empty()): 
            raise Exception("MeanQueue is empty")
            return
        val = self.data.dequeue()
        self.sum -= val 
        return val
    def first(self):
        ''' Return a reference to the first element of the queue
        without removing it. If the queue is empty, raise an
        exception '''
        return self.data.first()
    def sum(self):
        ''' Returns the sum of all values in the queue'''
        return self.sum 
    def mean(self):
        ''' Return the mean (average) value in the queue'''
        return self.sum/len(self.data)

class QueueStack:
    def __init__(self):
        self.data = ArrayQueue()
    def __len__(self):
        return len(self.data)
    def is_empty(self):
        return len(self) == 0
    def push(self, e):
        ''' Add element e to the top of the stack '''
        self.data.enqueue(e)
        for i in range(len(self.data)-1): 
            val = self.data.dequeue
            self.data.enqueue(val)

    def pop(self):
        ''' Remove and return the top element from the stack. If the stack
        is empty, raise an exception'''
        return self.data.dequeue()
    def top(self):
        pass
print("hello")
print(10 + 1%10)

def flatten_list_by_depth(lst):
    """
    : lst type: list
    : return type: list
    """
    q = ArrayQueue()
    new_lst = []
    counter = 0 
    for i in lst: 
        q.enqueue(i)
    while(not q.is_empty()): 
        current = q.dequeue()
        if(isinstance(current, list)): 
            for i in current: 
                q.enqueue(i)
        else: 
            new_lst.append(current)
    return new_lst

lst = [ [[[0]]], [1, 2], 3, [4, [5, 6, [7]], 8], 9]
new_lst = flatten_list_by_depth(lst)
print(new_lst) 
#[3, 9, 1, 2, 4, 8, 5, 6, 0, 7]
```