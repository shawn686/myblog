---
title: Searching a Value in BST Iteratively
date: 2019-04-25 09:15:46
tags:
- java
- datastructure
- tree
- BST
- search
- iterative
- fundamental
categories:
- [datastructure, tree, BST]
---

``` java
class TreeNode {
    public TreeNode(int val) { this.val = val; }
    int val;
    TreeNode left;
    TreeNode right;
}
public boolean searchBST(TreeNode root, int target) {
    TreeNode node = root;
    while (node != null) {
        if (node.val == target) {
            return true;
        } else if (target < node.val) {
            node = node.left;
        } else {
            node = node.right;
        }
    }
    return false;
}
```
