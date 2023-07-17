---
title: "HW6"
date: Homework
mainSectionTitle: "ptc"
---
[Download problem set](https://dk3156.github.io/experience/dsa/homeworks/dsa_hw6.pdf)
## Answers

## Q1
```
from DoublyLinkedList import * 
class LinkedQueue: 
    def __init__(self):
        self.data = DoublyLinkedList() 


    def __len__(self):
        return len(self.data)


    def is_empty(self):
        return self.data.is_empty() 


    def enqueue(self, elem):
        self.data.add_last(elem)


    def dequeue(self):
        if(self.is_empty()): 
            raise Exception("Queue is empty")
        return self.data.delete_first()


    def first(self):
        if(self.is_empty()): 
            raise Exception("Queue is empty")
        return self.data.header.next.data
```
## Q2
```
from DoublyLinkedList import * 
class Integer:
    def __init__(self, num_str):
        ''' Initializes an Integer object representing
        the value given in the string num_str'''
        self.data = DoublyLinkedList() 
        start = False 
        for i  in num_str: 
            if not(i == "0"):
                start = True 
            if(start == True ):
                self.data.add_last(int(i))
        

    def __add__(self, other):
        ''' Creates and returns an Integer object that
        represent the sum of self and other, also of
        type Integer'''
        carry = 0 
        cursor1 = self.data.trailer.prev
        cursor2 = other.data.trailer.prev
        res = ""
        while(cursor1 is not self.data.header and cursor2 is not other.data.header): 
            val = cursor1.data+cursor2.data+carry
            carry = 1 if( val != val%10) else 0 
            val = val%10 
            res = str(val)+res
            cursor1 =cursor1.prev
            cursor2 = cursor2.prev
        while(cursor1 is not self.data.header): 
            val = cursor1.data + carry 
            carry = 1 if( val != val%10) else 0 
            val = val%10 
            res = str(val)+res
            cursor1 =cursor1.prev
        
        while(cursor2 is not other.data.header):
            val = cursor2.data + carry 
            carry = 1 if( val != val%10) else 0 
            val = val%10 
            res = str(val)+res
            cursor2 =cursor2.prev
        if(carry == 1): 
            res = str(1)+res
        return Integer(res)

    def __mul__(self,other): 
        multi = []
        order1 = 10**(len(other.data)-1) 
        for i in other.data: 
            order2 = 10**(len(self.data)-1) 
            for j in self.data: 
                multi.append(Integer(str(int((i*order1)*(j*order2)))))
                order2 /= 10
            order1 /= 10 
        res = Integer("0")
        for i in multi: 
            res = res + i 
        return res 





    def __repr__(self):
        ''' Creates and returns the string representation
        of self'''
        res = ""
        for  i in self.data: 
            res += (str(i))
        return res
```

## Q3
```
from DoublyLinkedList import * 
class CompactString:
    def __init__(self, orig_str):
        ''' Initializes a CompactString object
        representing the string given in orig_str'''
        self.data = DoublyLinkedList()  
        val = 1
        current = ""
        for i in orig_str: 
            if(not(current == i)): 
                if(current != ""): 
                    self.data.add_last((current,val))
                current = i
                val =  1
            else: 
                val += 1 
        self.data.add_last((current,val))

    def __add__(self, other):
        ''' Creates and returns a CompactString object that
        represent the concatenation of self and other,
        also of type CompactString'''
        res = ""
        for i in self.data: 
            res += (i[0]*i[1])
        for i in other.data: 
            res += (i[0]*i[1])
        return CompactString(res)
    def __eq__(self,other): 
        if(len(self.data) != len(other.data)):  
            return False 
        cursor = other.data.header.next
        for i in self.data: 
            if(i[0] != cursor.data[0] or i[1] !=cursor.data[1]): 
                return False 
            cursor = cursor.next 
        if(cursor is not other.data.trailer): 
            return False
        return True 
    def __lt__(self, other):
        ''' returns True if”f self is lexicographically
        less than other, also of type CompactString'''
        # print(self)
        # print(other)
        cursor1 = self.data.header.next 
        length1 = cursor1.data[1]
        cursor2 = other.data.header.next
        length2 = cursor2.data[1]
        while(cursor1 is not self.data.trailer and cursor2 is not other.data.trailer): 
            data1 = cursor1.data 
            data2 = cursor2.data 
            # print(data1[0], data2[0])
            if(data1[0] < data2[0]): 
                return True
            elif(data1[0] > data2[0]): 
                return False 
            if(data1[1] > data2[1]): 
                cursor2 = cursor2.next
            elif(data1[1] < data2[1]): 
                cursor1 = cursor1.next 
            else: 
                cursor2 = cursor2.next
                cursor1 = cursor1.next 
        if(cursor1 is self.data.trailer and cursor2 is other.data.trailer): 
            return False 
        if(cursor1 is self.data.trailer): 
            return True 
        else: 
            return False 
            
        return False
    def __le__(self, other):
        ''' returns True if”f self is lexicographically
        less than or equal to other, also of type
        CompactString'''
        return ( self < other or self == other)
    def __gt__(self, other):
        ''' returns True if”f self is lexicographically
        greater than other, also of type CompactString'''
        return not(self < other)
    def __ge__(self, other):
        ''' returns True if”f self is lexicographically
        greater than or equal to other, also of type
        CompactString'''
        return (self > other or self == other)

    def __repr__(self):
        ''' Creates and returns the string representation
        (of type str) of self'''
        res = ""
        for i in self.data: 
            res += (i[0]*i[1])
        return res
```

## Q4
```
from DoublyLinkedList import * 
def copy_linked_list(lnk_lst): 
    copy = DoublyLinkedList()
    for i in  lnk_lst: 
        copy.add_last(i)
    return copy 

def deep_copy_linked_list(lnk_lst):
    if(not isinstance(lnk_lst, DoublyLinkedList)): 
        return lnk_lst
    copy = DoublyLinkedList()
    for i in  lnk_lst: 
        val = deep_copy_linked_list(i)
        copy.add_last(val)
    return copy 
```

## Q5
```
from DoublyLinkedList import * 
def merge_linked_lists(srt_lnk_lst1, srt_lnk_lst2):
    def merge_sublists(node1, node2, new_lst ):
        if(node1.data == None): 
            new_lst.add_last(node2.data)
            return new_lst

        elif(node2.data == None): 
            new_lst.add_last(node1.data)
            return new_lst
        if(node1.data > node2.data): 
            new_lst.add_last(node2.data)
            node2 = node2.next  
        else: 
            new_lst.add_last(node1.data)
            node1 = node1.next 
        merge_sublists(node1,node2, new_lst) 
        return new_lst
    new_lst =  DoublyLinkedList() 
    return merge_sublists(srt_lnk_lst1.header.next,  srt_lnk_lst2.header.next,  new_lst)
```