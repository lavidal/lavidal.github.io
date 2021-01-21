---
title: Binary Tree Traversal
date: 2020-08-22 14:55:16
tags:
- Algorithm
categories:
- Algorithm
---

In this article we will discuss two approaches to do binary tree traversal: recursion based and stack based.

In general, these two approach have their own strengths and weaknesses. The recursion ones are easy to understand and implement however might cause stack overflow. The stack based are more efficient in terms of memory but hard to understand and more complex to implement compared to recursion based approach. Normally on a coding interview, the interviewer prefers stack based binary tree traversal.

We will discuss four types of binary tree traversal:

- Preorder Traversal https://leetcode.com/problems/binary-tree-preorder-traversal/
- Inorder Traversal https://leetcode.com/problems/binary-tree-inorder-traversal/
- Postorder Traversal https://leetcode.com/problems/binary-tree-postorder-traversal/
- Level Order Traversal https://leetcode.com/problems/binary-tree-level-order-traversal/

### Binary Tree Preorder Traversal

#### Recursion based

```python
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        self.helper(root, res)
        return res
    
    def helper(self, root, res):
        if not root: return
        
        res.append(root.val)
        self.helper(root.left, res)
        self.helper(root.right, res)
        return
```

#### Stack based:

##### How to derive a stack-based solution?

For preorder traversal, it's root->left->right. So for any node, we first get its value and then check if it has left child. If so, we goes into left child. Until it's none. If it's none, we want to check it's right child. 

Remember after visited left child we want to visit its right child. That why we need stack. 

"For each node, it is root to itself."

For any node the detailed steps are:

1) Get its value, store the node into stack and goes to its left child

2) If left child is none then pop the stack and goes into its right child. If left child is valid then continue step 1

Universal one:

```python
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        stk = []
        res = []
        tmp = root
        while tmp or stk:
            if tmp:
                res.append(tmp.val)
                stk.append(tmp)
                tmp = tmp.left
            else:
                tmp = stk.pop()
                tmp = tmp.right
        return res
```

Special yet simple one:

```python
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        if not root: return []
        res = []
        stk = [root]
        while stk:
            tmp = stk.pop()
            res.append(tmp.val)
            if tmp.right:
                stk.append(tmp.right)
            if tmp.left:
                stk.append(tmp.left)
        return res
```

### Binary Tree Inorder Traversal

#### Recursion based:

```python
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        self.helper(root, res)
        return res
    
    def helper(self, root, res):
        if not root: return
        self.helper(root.left, res)
        res.append(root.val)
        self.helper(root.right, res)
        return
```

#### Stack based:

##### How to derive a stack-based solution?

For inorder traversal, it's left->root->right. So for any node, we check if it has left child. If so, we goes into left child. Until it's none. If it's none, we want to get back to node gets its value and check it's right child. 

Remember after visited left child we want to visit itself.

"For each node, it is root to itself."

For any node the detailed steps are:

1) Store the node into stack and goes to its left child

2) If left child is none then pop the stack, gets the node's value and  goes into its right child. If left child is valid then continue step 1

```python
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        res, stk = [], []
        tmp = root
        while tmp or stk:
            if tmp:
                stk.append(tmp)
                tmp = tmp.left
            else:
                tmp = stk.pop()
                res.append(tmp.val)
                tmp = tmp.right
        
        return res
```

### Binary Tree Postorder Traversal

#### Recursion based:

```python
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        self.helper(root, res)
        return res
    
    def helper(self, root, res):
        if not root: return
        self.helper(root.left, res)
        self.helper(root.right, res)
        res.append(root.val)
        return
```

#### Stack based:

##### How to derive a stack-based solution?

Can't use the universal template for preorder and inorder. Because it's left->right->root, the root is the last one to visit. It's useless to use stack. Because when we pop a node from stack, it's root to itself.

This one is harder than others. But we have a tricky solution. We just traverse tree in root-right-left order. And return the reversed result is the correct answer!

```python
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        res, stk = [], []
        tmp = root
        while tmp or stk:
            if tmp:
                res.append(tmp.val)
                stk.append(tmp)
                tmp = tmp.right
            else:
                tmp = stk.pop()
                tmp = tmp.left
        
        return res[::-1]
```

### Binary Tree Level Order Traversal

This one we use queue and breadth first search instead. 

Recommend you look https://lavidal.github.io/2020/08/06/Breadth-First-Search/ for the idea and solution!