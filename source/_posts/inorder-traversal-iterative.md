---
title: Binary Tree Inorder Traversal Iterative
date: 2019-04-22 09:23:16
tags:
- java
- datastructure
- tree
- binary tree
- traversal
- iterative
- fundamental
- stack
categories:
- [datastructure, tree, traversal]
---

### Recursive way is trivial but iterative takes some time.
Both recursive and iterative have a **space complexity** of **O(log(N))** if the tree is well balanced while the worst case is **O(N)** when the tree is linear e.g.:

            3
           /
          2
         /
        1
Both **time complexities** are __O(N)__ as they go over all the nodes.


``` java
class TreeNode {
    public TreeNode(int val) {
        this.val = val;
    }
    int val;
    TreeNode left;
    TreeNode right;
}

public void inorder_iter(TreeNode root) {
    // 1. if cur node not null then push cur to a stack and make left node cur node
    // 2. if cur == null, stack pop and visit
    // 3. cur node = {popped node right}
    Stack<TreeNode> stk = new Stack<>();
    while (true) {
        // go to the left leaf node
        while (root != null) {
            stk.push(root);
            root = root.left;
        }
        if (stk.isEmpty()) {
            return;
        }
        root = stk.pop();
        // visit the node
        System.out.println(root.val);
        // visit the right sub-tree
        root = root.right;
    }
}
/**
          8
        /   \
      6      10
    /   \      \
   5     7     12
              /
            11
**/
```
