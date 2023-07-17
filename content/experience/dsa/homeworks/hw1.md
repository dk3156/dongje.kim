---
title: "HW1"
date: Homework
mainSectionTitle: "ptc"
---
[Download problem set](https://dk3156.github.io/experience/dsa/homeworks/dsa_hw1.pdf)
## Answers

## Q2
```
# part a
def shift(lst, k):
    lst = lst[k:]+lst[:k]

# part b
def shift2(lst, k, dir = 'left'):
    if dir == 'right':
        k = len(lst) - k
    lst[:] = lst[k:]+lst[:k]

def main():
    lst = [1, 2, 3, 4, 5, 6]
    shift(lst, 2)
    print(lst)

    lst2 = [1, 2, 3, 4, 5, 6]
    shift2(lst2, 2, 'right')
    print(lst2)

    lst1 = [1,2,3]
    lst2 = lst1[1:]
    print(lst1)
    print(lst2)

main()
```
## Q3
```
# part a
def sum_sqr(n):
    result = 0
    for i in range(1, n):
        result += i**2
    return result

# part b
sum([i**2 for i in range(1, n)])

# part c
def sum_odd(n):
    result = 0
    for i in range(1, n):
        if i%2 == 1:
            result += i**2
    return result

# part d
sum([i**2 for i in range(1, n) if i%2 == 1])
```

## Q4
```
# part a
[ 10**i for i in range(6) ]

# part b
[ i*(i+1) for i in range(10) ]

# part c
[ chr(i) for i in range(ord('a'),ord('z')+1) ]
```

## Q5
```
# part a
[ 10**i for i in range(6) ]

# part b
[ i*(i+1) for i in range(10) ]

# part c
[ chr(i) for i in range(ord('a'),ord('z')+1) ]
```

## Q6
```
def fibs(n):
    first = 0
    second = 1
    yield 1
    for i in range(n-1):
        result = first + second
        first = second
        second = result
        yield result

def main():
    for curr in fibs(8):
        print(curr, end=" ")

#main()
```

## Q7
```
class Vector:
    # part a
    def __init__(self, d):
        if isinstance(d,int) == True:
            self.coords = [0]*d
        else:
            self.coords = list(d)

    def __len__(self):
        return len(self.coords)

    def __getitem__(self, j):
        return self.coords[j]

    def __setitem__(self, j, val):
        self.coords[j] = val

    def __add__(self, other):
        if (len(self) != len(other)):
            raise ValueError ("dimensions must agree")
        result = Vector(len(self))
        for j in range(len(self)):
            result[j] = self[j] + other[j]
        return result

    # part b
    def __sub__(self, other):
        if (len(self) != len(other)):
            raise ValueError ("dimensions must agree")
        result = Vector(len(self))
        for j in range(len(self)):
            result[j] = self[j] - other[j]
        return result

    # part c
    def __neg__(self):
        result = []
        for ele in self.coords:
            result.append(-1 * ele)
        return Vector(result)

    # part d & f
    def __mul__(self, other):
        if isinstance(other,int) == True:
            result = []
            for ele in self.coords:
                result.append(other * ele)
            return Vector(result)
        else:
            result = 0
            for i in range(len(self.coords)):
                result += (self.coords[i] * other.coords[i])
            return result

    # part e & f
    def __rmul__(self, other):
        result = []
        for ele in self.coords:
            result.append(other * ele)
        return Vector(result)

    def __eq__(self, other):
        return self.coords == other.coords

    def __ne__(self, other):
        return not (self == other)

    def __str__(self):
        return '<'+ str(self.coords)[1:-1] + '>'

    def __repr__(self):
        return str(self)

def main():
    # part a testcode
    v1 = Vector(5)
    v1[1] = 10
    v1[-1] = 10
    print(v1)

    v2 = Vector([2, 4, 6, 8, 10])
    print(v2)

    # part b testcode
    u1 = v1 - v2
    print(u1)

    # part c testcode
    u2 = -v2
    print(u2)

    # part d testcode
    u4 = v2 * 3
    print(u4)

    # part e testcode
    u3 = 3 * v2
    print(u3)

    # part f testcode
    u5 = v1 * v2
    print(u5)

#main()
```