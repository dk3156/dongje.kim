---
title: "DSA lab 10"
date: Labs
mainSectionTitle: "ptc"
---
Problem set is not available.
## Answer for Coding Questions
```
from DoublyLinkedList import * 
class LinkedStack: 
    def __init__(self): 
        self.data = DoublyLinkedList() 
    def __len__(self):
        ''' Returns the number of elements in the stack. '''
        return len(self.data)
    def is_empty(self):
        ''' Returns true if the stack is empty,false otherwise.
        '''
        return self.data.is_empty()
    def push(self, e):
        ''' Adds an element, e, to the top of the stack. '''
        self.data.add_first(e)
    def top(self):
        ''' Returns the element at the top of the stack.
        An exception is raised if the stack is empty. '''
        return self.data.header
    def pop(self):
        ''' Removes and returns the element at the top of the
        stack.
        An exception is raised if the stack is empty. '''
        return self.data.delete_first()

class MidStack:
def __ init__(self):
    self.data = DoublyLinkedList( )
    self.mid = None
    def __len__(self):
        ''' Returns the number of elements in the stack. '''
        return len(self.data)
    def is_empty(self):
        ''' Returns true if stack is empty and false otherwise.
        '''
        return self.data.is_empty() 
    def push(self, e):
        ''' Adds an element, e, to the top of the stack. '''
        self.data.add_first(e)
        if(len(self.data) % 2 == 1): 
            self.mid = self.data.header if self.mid == None else self.mid.next
    def top(self):
        ''' Returns the element at the top of the stack.
        An exception is raised if the stack is empty. '''
        return self.data.header
    def pop(self):
        ''' Removes and returns the element at the top of the
        stack.
        An exception is raised if the stack is empty. '''

        val = self.data.delete_first()
        if(len(self.data) % 2 == 0): 
            self.mid = self.mid.prev
        return val 
    def mid_push(self, e):
        ''' Adds an element, e, to the middle of the stack.'''
        nex = self.mid.next 
        self.mid.next = DoublyLinkedList.Node(e)
        self.mid.next = nex 
        if(len(self.mid) % 2 = 1):
            self.mid = self.mid.next 
```