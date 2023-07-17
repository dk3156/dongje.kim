---
title: "DSA lab 10"
date: Labs
mainSectionTitle: "ptc"
---
Problem set is not available.
## Answer for Coding Questions
```
from LinkedBinaryTree import * 

def bt_even_sum(root): 
    if(root == None): 
        return 0    
    val = bt_even_sum(root.left)+bt_even_sum(root.right)
    if(root.data % 2 == 0): 
        return root.data + val 
    else: 
        return val 

def bt_contains(root,val): 
    if(root == None): 
        return False 
    if(root.data == val): 
        return True 
    else: 
        return (bt_contains(root.left, val) or bt_contains(root.right, val))
    

def add_bts(root1, root2): 
    ''' Creates a new binary tree merging tree1 and tree2
    and returns its root. '''
    if(root1 == None and root2 == None): 
        return
    val = (root1.data if root1 != None else 0) + (root2.data if root2 != None else 0)
    root = LinkedBinaryTree.Node(val, add_bts(root1.left if root1 != None else None, root2.left if root2 != None else None), add_bts(root1.right if root1 != None else None, root2.right if root2 != None else None))
    return root 

def invert_bt(root):
    ''' Inverts the binary tree using recursion '''
    def invert_bt_helper(roots): 
        if(len(roots) == 0): 
            return 
        for i in range(len(roots)//2): 
            holder = roots[i].data 
            roots[i].data = roots[len(roots)-1-i].data 
            roots[len(roots)-1-i].data = holder
        new_roots = []
        for i in roots: 
            new_roots.append(roots.left) 
            new_roots.append(roots.rght)
        invert_bt_helper(new_roots)

    roots = [root] 
    invert_bt_helper(roots)

def invert_bt(root):
    ''' Inverts the binary tree without recursion '''
    roots = [root]
    while(len(roots) != 0): 
        for i in range(len(roots)//2): 
            holder = roots[i].data 
            roots[i].data = roots[len(roots)-1-i].data 
            roots[len(roots)-1-i].data = holder
        new_roots = []
        for i in roots: 
            new_roots.append(roots.left) 
            new_roots.append(roots.rght)
        roots = new_roots

def is_complete(root):
    ''' Returns True if the Binary Tree is complete and
    false if not ''' 
```