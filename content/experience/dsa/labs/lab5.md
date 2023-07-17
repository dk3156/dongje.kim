---
title: "DSA lab 5"
date: Labs
mainSectionTitle: "ptc"
---

[Download problem set](https://dk3156.github.io/experience/dsa/labs/dsa_lab5.docx)
## Answer for Coding Questions
```
# Coding Question

import ctypes  # provides low-level arrays
def make_array(n):
    return (n * ctypes.py_object)()

#Start Code, except for #g
class ArrayList:

    def __init__(self, iter_collection = None):
        self.data = make_array(1)
        self.n = 0
        self.capacity = 1

        #g
        if iter_collection is not None:
            for elem in iter_collection:
                self.append(elem)


    def append(self, val):
        if(self.n == self.capacity):
            self.resize(2 * self.capacity)
        self.data[self.n] = val
        self.n += 1

    def resize(self, new_size):
        new_arr = make_array(new_size)
        for i in range(self.n):
            new_arr[i] = self.data[i]
        self.data = new_arr
        self.capacity = new_size

    def __len__(self):
        return self.n


#d
    def __getitem__(self, ind):
        if (not (-self.n <= ind <= self.n - 1)):
            raise IndexError('invalid index')
        
        if (ind < 0):
            ind = self.n + ind
        return self.data[ind]

#d
    def __setitem__(self, ind, val):
        if (not (-self.n <= ind <= self.n - 1)):
            raise IndexError('invalid index')
        
        if (ind < 0):
            ind = self.n + ind
        self.data[ind] = val


    def extend(self, iter_collection):
        for elem in iter_collection:
            self.append(elem)


    def __iter__(self):
        for i in range(self.n):
            yield self.data[i]




#LAB PORTION

#a

    def __repr__(self):
        return ("[" + ", ".join("'"+val+"'" if isinstance(val, str) else str(val) for val in self) + "]") #the if statement adds ' ' if type is str else convert it to str( )


#b

    def __add__(self, other):
        lst  = ArrayList()
        lst  += self 
        lst  += other
        return lst 

#c

    def __iadd__(self, other):
        self.extend(other)
        return self


#e

    def __mul__(self, scalar): #scalar is a multiplicative constant
        lst = ArrayList()
        for i in range(scalar): #k
            lst.extend(self) #n --> total runtime = k*n

        return lst

#f

    def __rmul__(self, scalar):
        return self * scalar


#h 
    def remove(self, val):
        #first find the index at which  arr[i] == val
        i = 0

        #while index is in range and elem at index i isn't val
        while i < self.n and self[i] != val:
            i += 1
        
        while i < self.n - 1:
            self[i] = self[i + 1]
            i += 1
        
        if i < self.n: #i should be in the last index (len - 1). The condition is if i is still within range/ list is not empty.
            self[i] = None                                      
            self.n -= 1

    
#i 
    #we've actually done this before with move_zeroes in Lab 3
    def removeAll(self, val):

        #first move all instances of val to the back
        last_val = 0                   #keep track of the last zero
        for i in range(len(self)):      #use i to traverse through the list, 
            if self[i] != val:            #if lst[i] != 0, swap, then move the last zero reference up
                self[i], self[last_val] = self[last_val], self[i]
                last_val += 1

        while self[i] == val:
           self[i] = None
           i -= 1
           self.n -= 1 #don't forget to decrement the size

'''

arr = ArrayList("Python")
print(arr)


for i in range(len(arr)):
    print(arr[i], end = " ")
    arr[i] *= 2

print( )
for i in range(-1, -len(arr)-1, -1):
    print(arr[i], end = " ")

print()
arr += ArrayList(("Python"))
print(arr)

for i in range(-1, -len(arr)-1, -1):
    print(arr[i])


arr = ArrayList([1, 2, 3, 2, 3, 4, 2, 2])
arr.removeAll(2)
print(arr, len(arr))

arr.removeAll(3)
print(arr, len(arr))

arr.removeAll(5)
print(arr, len(arr))

arr = ArrayList([1, 2, 3, 2, 3, 4, 2, 2])
arr.remove(4)
print(arr, len(arr))

arr.remove(3)
print(arr, len(arr))

arr.removeAll(2)
print(arr, len(arr))

arr += [5, 5, 5]
print(arr, len(arr))
print(arr * 3)
'''
```