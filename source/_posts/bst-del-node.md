---
title: LeetCode 450. Delete Node in a BST
date: 2019-04-28 20:13:45
tags:
- java
- datastructure
- tree
- BST
- leetcode
- medium
categories:
- [datastructure, tree, BST]
---
![Successor and predecessor.](https://user-images.githubusercontent.com/24547983/56872172-f59a2480-69f3-11e9-9164-b58e03cbf8f6.png)

![Node is a leaf, and one could delete it straightforward.](https://user-images.githubusercontent.com/24547983/56872228-92f55880-69f4-11e9-98c7-0fe0895159d8.png)

![Node is not a leaf and has a right child.](https://user-images.githubusercontent.com/24547983/56872254-dfd92f00-69f4-11e9-8bbc-20959f6e1df4.png)

![Node is not a leaf, has no right child and has a left child.](https://user-images.githubusercontent.com/24547983/56872264-f8494980-69f4-11e9-9c14-ca79a120d007.png)

``` java
class TreeNode {
    TreeNode(int val) { this.val = val; }
    int val;
    TreeNode left;
    TreeNode right;
}

int successor(TreeNode root) {
    root = root.right;
    while (root.left != null) root = root.left;
    return root.val;
}

int predecessor(TreeNode root) {
    root = root.left;
    while (root.right != null) root = root.right;
    return root.val;
}

public TreeNode deleteNode(TreeNode root, int key) {
    if (root == null) return null;
    if (key < root.val) root.left = deleteNode(root.left, key);
    else if (key > root.val) root.right = deleteNode(root.right, key);
    else {
        // Node is a leaf, and one could delete it straightforward.
        if (root.left == null && root.right == null) {
            root = null;
        }
        // Node has right child. Find the successor, set root value with successor's value and recursive call from the right child
        else if (root.right != null) {
            root.val = successor(root);
            root.right = deleteNode(root.right, root.val);
        }
        // Node has left child. Find the predecessor, set root value with predecessor's value and recursive call from the left child
        else {
            root.val = predecessor(root);
            root.left = deleteNode(root.left, root.val);
        }
    }
    return root;
}

```