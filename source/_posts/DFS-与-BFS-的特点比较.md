---
title: DFS 与 BFS 的特点比较
date: 2020-11-01 23:01:50
tags: binary tree
category: Algorithm
toc: true
---

# DFS 与 BFS 的特点比较

### BFS 的适用场景
### 如何用 BFS 进行层序遍历
### 如何用 BFS 求解最短路径问题

# 运用递归解决树的问题

* [给定一个二叉树，请寻找它的最大深度。  ](https://leetcode-cn.com/leetbook/read/data-structure-binary-tree/xefb4e/)
* [判断二叉树是否对称](https://leetcode-cn.com/problems/symmetric-tree/)
	* 递归解法： 双指针判断
	* 迭代解法：层序遍历，根节点入队列两次，然后每次拉出来两个节点对比是否相等

## 前序遍历（自顶向下）
根节点的深度是1。
depth = 1, answer = 0;

```
private int answer;
private void maximum_depth(TreeNode root, int depth) {
    if (root == null) {
        return;
    }
    if (root.left == null && root.right == null) {
        answer = Math.max(answer, depth);
    }
    maximum_depth(root.left, depth + 1);
    maximum_depth(root.right, depth + 1);
}
```

## 后序遍历（自底向上）
```java
public int maximum_depth(TreeNode root) {
	if (root == null) {
		return 0;                                   // return 0 for null node
	}
	int left_depth = maximum_depth(root.left);
	int right_depth = maximum_depth(root.right);
	return Math.max(left_depth, right_depth) + 1;	// return depth of the subtree rooted at root
}
```
