---
title: "HW7"
date: Homework
mainSectionTitle: "ptc"
---
[Download problem set](https://dk3156.github.io/experience/dsa/homeworks/dsa_hw7.pdf)
## Answers

## Q1
```
from LinkedBinaryTree import LinkedBinaryTree

def min_and_max(bin_tree):
    def subtree_min_and_max(root): 
        if(root == None): 
            return None 
        min_max = (root.data, root.data)
        left = subtree_min_and_max(root.left) 
        right = subtree_min_and_max(root.right)
        if(left is not None): 
            min_max = (min(min_max[0],left[0]), max(min_max[1], left[1])   )
        if(right is not None):
            min_max = (min(min_max[0],right[0]), max(min_max[1], right[1])   )
        # print(min_max)
        return min_max
    if(bin_tree is None): 
        raise Exception("None Input")
    if(bin_tree.is_empty()): 
        raise Exception("Tree is empty")

        
    
    return subtree_min_and_max(bin_tree.root)
```
## Q2
```
from ArrayQueue import ArrayQueue

class LinkedBinaryTree:

    class Node:
        def __init__(self, data, left=None, right=None):
            self.data = data
            self.parent = None
            self.left = left
            if (self.left is not None):
                self.left.parent = self
            self.right = right
            if (self.right is not None):
                self.right.parent = self

    def __init__(self, root=None):
        self.root = root
        self.size = self.count_nodes()

    def __len__(self):
        return self.size

    def is_empty(self):
        return len(self) == 0


    def count_nodes(self):
        def subtree_count(root):
            if (root is None):
                return 0
            else:
                left_count = subtree_count(root.left)
                right_count = subtree_count(root.right)
                return 1 + left_count + right_count

        return subtree_count(self.root)


    def sum(self):
        def subtree_sum(root):
            if (root is None):
                return 0
            else:
                left_sum = subtree_sum(root.left)
                right_sum = subtree_sum(root.right)
                return root.data + left_sum + right_sum

        return subtree_sum(self.root)


    def height(self):
        def subtree_height(root):
            if (root.left is None and root.right is None):
                return 0
            elif (root.left is None):
                return 1 + subtree_height(root.right)
            elif (root.right is None):
                return 1 + subtree_height(root.left)
            else:
                left_height = subtree_height(root.left)
                right_height = subtree_height(root.right)
                return 1 + max(left_height, right_height)

        if(self.is_empty()):
            raise Exception("Tree is empty")
        return subtree_height(self.root)


    def preorder(self):
        def subtree_preorder(root):
            if (root is None):
                pass
            else:
                yield root
                yield from subtree_preorder(root.left)
                yield from subtree_preorder(root.right)

        yield from subtree_preorder(self.root)


    def postorder(self):
        def subtree_postorder(root):
            if (root is None):
                pass
            else:
                yield from subtree_postorder(root.left)
                yield from subtree_postorder(root.right)
                yield root

        yield from subtree_postorder(self.root)


    def inorder(self):
        def subtree_inorder(root):
            if (root is None):
                pass
            else:
                yield from subtree_inorder(root.left)
                yield root
                yield from subtree_inorder(root.right)

        yield from subtree_inorder(self.root)


    def breadth_first(self):
        if (self.is_empty()):
            return
        line = ArrayQueue()
        line.enqueue(self.root)
        while (line.is_empty() == False):
            curr_node = line.dequeue()
            yield curr_node
            if (curr_node.left is not None):
                line.enqueue(curr_node.left)
            if (curr_node.right is not None):
                line.enqueue(curr_node.right)

    def __iter__(self):
        for node in self.breadth_first():
            yield node.data
    def leaves_list(self):
        
        def leaves_list_helper(root, ans): 
            if(root == None): 
                return 
            if(root.left == None and root.right == None): 
                ans.append(root.data)
            leaves_list_helper(root.left, ans)
            leaves_list_helper(root.right, ans)
            return 
        ans = [] 
        leaves_list_helper(self.root, ans)
        return ans
    def _gen_leaves(self, parent): 
        if(parent == None): 
            return 
        if(parent.left == None and parent.right == None): 
            yield parent.data 
        print(parent.left)
        print(parent.right)
        self._gen_leaves(parent.left)
        self._gen_leaves(parent.right)
```

## Q3
```
from LinkedBinaryTree import LinkedBinaryTree
def is_height_balanced(bin_tree):
    def subtree_height(root): 
        if(root == None): 
            return (0,True) 
        left_height = subtree_height(root.left)
        right_height = subtree_height(root.right)
        isbalanced = left_height[1] and right_height[1]
        if(abs(left_height[0]-right_height[0]) > 1): 
            isbalanced = False
        return (max(left_height[0], right_height[0])+1, isbalanced)
    return subtree_height(bin_tree.root)[1]
```

## Q4
```
from ArrayQueue import ArrayQueue

class LinkedBinaryTree:

    class Node:
        def __init__(self, data, left=None, right=None):
            self.data = data
            self.parent = None
            self.left = left
            if (self.left is not None):
                self.left.parent = self
            self.right = right
            if (self.right is not None):
                self.right.parent = self

    def __init__(self, root=None):
        self.root = root
        self.size = self.count_nodes()

    def __len__(self):
        return self.size

    def is_empty(self):
        return len(self) == 0


    def count_nodes(self):
        def subtree_count(root):
            if (root is None):
                return 0
            else:
                left_count = subtree_count(root.left)
                right_count = subtree_count(root.right)
                return 1 + left_count + right_count

        return subtree_count(self.root)


    def sum(self):
        def subtree_sum(root):
            if (root is None):
                return 0
            else:
                left_sum = subtree_sum(root.left)
                right_sum = subtree_sum(root.right)
                return root.data + left_sum + right_sum

        return subtree_sum(self.root)


    def height(self):
        def subtree_height(root):
            if (root.left is None and root.right is None):
                return 0
            elif (root.left is None):
                return 1 + subtree_height(root.right)
            elif (root.right is None):
                return 1 + subtree_height(root.left)
            else:
                left_height = subtree_height(root.left)
                right_height = subtree_height(root.right)
                return 1 + max(left_height, right_height)

        if(self.is_empty()):
            raise Exception("Tree is empty")
        return subtree_height(self.root)


    def preorder(self):
        def subtree_preorder(root):
            if (root is None):
                pass
            else:
                yield root
                yield from subtree_preorder(root.left)
                yield from subtree_preorder(root.right)

        yield from subtree_preorder(self.root)


    def postorder(self):
        def subtree_postorder(root):
            if (root is None):
                pass
            else:
                yield from subtree_postorder(root.left)
                yield from subtree_postorder(root.right)
                yield root

        yield from subtree_postorder(self.root)


    def inorder(self):
        def subtree_inorder(root):
            if (root is None):
                pass
            else:
                yield from subtree_inorder(root.left)
                yield root
                yield from subtree_inorder(root.right)

        yield from subtree_inorder(self.root)


    def breadth_first(self):
        if (self.is_empty()):
            return
        line = ArrayQueue()
        line.enqueue(self.root)
        while (line.is_empty() == False):
            curr_node = line.dequeue()
            yield curr_node
            if (curr_node.left is not None):
                line.enqueue(curr_node.left)
            if (curr_node.right is not None):
                line.enqueue(curr_node.right)

    def __iter__(self):
        for node in self.breadth_first():
            yield node.data
    def leaves_list(self):
        
        def leaves_list_helper(root, ans): 
            if(root == None): 
                return 
            if(root.left == None and root.right == None): 
                ans.append(root.data)
            leaves_list_helper(root.left, ans)
            leaves_list_helper(root.right, ans)
            return 
        ans = [] 
        leaves_list_helper(self.root, ans)
        return ans

    def iterative_inorder(self):
        current = self.root 
        while(current != None): 
            if(current.left == None): 
                yield current.data 
                current = current.right 
            else: 
                nextVal = current.left 
                while(nextVal.right != None): 
                    if(nextVal.right == current): 
                        break
                    nextVal = nextVal.right 
                if(nextVal.right != None): 
                    nextVal.right = None 
                    yield current.data 
                    current = current.right 
                else: 
                    nextVal.right = current 
                    current = current.left 
```

## Q5
```
from LinkedBinaryTree import LinkedBinaryTree
def create_expression_tree(prefix_exp_str): 
    def create_expression_helper(token_itr):
        token = next(token_itr, None)
        # print(token)
        if(token == None): 
            return 
        if(token not in "+-*/"): 
            return LinkedBinaryTree.Node(int(token))
        root = LinkedBinaryTree.Node(token)
        root.left = create_expression_helper(token_itr)
        root.right = create_expression_helper(token_itr)
        return root
    tokens = prefix_exp_str.split() 
    token_itr = iter(tokens)
    root = create_expression_helper(token_itr)
    return LinkedBinaryTree(root)
# tree = 	create_expression_tree('* 2 + - 15 6 4')
# print()

def prefix_to_postfix(prefix_exp_str): 
    tree = create_expression_tree(prefix_exp_str)
    res = ""
    for i in tree.postorder(): 
        res += str(i.data)+" "
    return res[:len(res)-1]
```