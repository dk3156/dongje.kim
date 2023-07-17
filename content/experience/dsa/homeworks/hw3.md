---
title: "HW3"
date: Homework
mainSectionTitle: "ptc"
---
[Download problem set](https://dk3156.github.io/experience/dsa/homeworks/dsa_hw3.pdf)
## Answers

## Q2
```
import ctypes  # provides low-level arrays
def make_array(n):
    return (n * ctypes.py_object)()

class ArrayList:
    def __init__(self):
        self.data = make_array(1)
        self.capacity = 1
        self.n = 0

    def __len__(self):
        return self.n


    def append(self, val):
        if (self.n == self.capacity):
            self.resize(2 * self.capacity)
        self.data[self.n] = val
        self.n += 1

    def resize(self, new_size):
        new_array = make_array(new_size)
        for i in range(self.n):
            new_array[i] = self.data[i]
        self.data = new_array
        self.capacity = new_size

    def extend(self, iter_collection):
        for elem in iter_collection:
            self.append(elem)


    def __getitem__(self, ind):
        if (not (-self.n <= ind <= self.n - 1)):
            raise IndexError('invalid index')
        if (ind < 0):
            ind = self.n + ind
        return self.data[ind]

    def __setitem__(self, ind, val):
        if (not (-self.n <= ind <= self.n - 1)):
            raise IndexError('invalid index')
        if (ind < 0):
            ind = self.n + ind
        self.data[ind] = val


    def __repr__(self):
        data_as_strings = [str(self.data[i]) for i in range(self.n)]
        return '[' + ', '.join(data_as_strings) + ']'


    def __add__(self, other):
        res = ArrayList()
        res.extend(self)
        res.extend(other)
        return res

    def __iadd__(self, other):
        self.extend(other)
        return self

    def __mul__(self, times):
        res = ArrayList()
        for i in range(times):
            res.extend(self)
        return res

    def __rmul__(self, times):
        return self * times

    # above are codes copied from given file

    def insert(self, index, val):
        if (not (-self.n <= index <= self.n - 1)):
            raise IndexError('invalid index')
        if (index < 0):
            index = self.n + index
        self.append(val) # append will take care of the capacity
        for i in range(self.n - index -1):
            self[self.n-i-1], self[self.n-i-2] = self[self.n-i-2], self[self.n-i-1]
        return self

    def pop(self, index = -1):
        if (not (-self.n <= index <= self.n - 1)):
            raise IndexError('invalid index')
        if (index < 0):
            index = self.n + index

        res = self[index]
        for i in range(index, self.n -1):
            self[i], self[i+1] = self[i+1], self[i]

        self[self.n -1] = None
        self.n -= 1

        if self.n < (self.capacity // 4):
            self.capacity = int(self.capacity //2)

        return res

def main():

    arr_lst = ArrayList()
    for i in range(1, 5):
        arr_lst.append(i)

    arr_lst.insert(2, 30)
    print(arr_lst)

    pop = arr_lst.pop(2)
    print(pop)
    print(arr_lst)

    pop = arr_lst.pop()
    print(arr_lst, pop)
    pop = arr_lst.pop()
    print(arr_lst, pop)
    pop = arr_lst.pop(0)
    print(arr_lst, pop)
    pop = arr_lst.pop(0)
    print(arr_lst, pop)

#main()
```
## Q3
```
def find_duplicates(lst):
    n = len(lst)
    test = [0]*n
    res = []
    for i in range(n):
        test[lst[i]] += 1
        if test[lst[i]] == 2:
            res.append(lst[i])
    return res

def main():
    lst = [2, 4, 4, 4, 1, 2]
    print(find_duplicates(lst))

#main()
```

## Q4
```
def remove_ind(lst, ind_lst):
    n = len(lst)
    counter = 0
    curr_ind = 0
    
    for i in range(n):
        if curr_ind >= len(ind_lst):
            lst[counter] = lst[i]
            counter += 1
        elif i != ind_lst[curr_ind]:
            lst[counter] = lst[i]
            counter += 1
        else:
            curr_ind += 1
    for j in range(n - counter):
        lst.pop()

def main():
    lst = [1, 2, 2, 3, 4, 5, 2, 6]
    remove_ind(lst, [0,1,2])
    print(lst)

main()
```