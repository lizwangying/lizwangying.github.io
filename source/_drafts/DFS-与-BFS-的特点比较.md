---
title: DFS 与 BFS 的特点比较
date: 2020-11-01 23:01:50
tags: binary tree
category: Algorithm
toc: true
---

树是非线性结构，对于存储一棵树不需要大块的连续内存，有利于提高计算机资源利用率。在实际生活场景中，深度优先搜索（DFS）和广度优先搜（BFS）索分别有哪些使用场景呢？

# BFS
* 实际场景：BFS 求解 `最短路径` 或者 `二分图判断` 、`最小生成树` ；
* 使用 `队列` 实现，一般会借助 `哈希表` 来优化时间复杂度；


# DFS
* 实际场景：文件目录的遍历，查找符合要求的文件。
*  一般使用 `栈` 实现，递归解法较容易实现；
* 和树的深度成正比，如果树比较深的话建议用 BFS。

# 使用 DFS 深度优先遍历
* [给定一个二叉树，请寻找它的最大深度。  ](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)
给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

说明: 叶子节点是指没有子节点的节点。

示例：
给定二叉树 [3,9,20,null,null,15,7]，

```
    3
   / \
  9  20
    /  \
   15   7
```
返回它的最大深度 3 。

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        } else {
            int leftHeight = maxDepth(root.left);
            int rightHeight = maxDepth(root.right);
            return Math.max(leftHeight, rightHeight) + 1;
        }
    }
}

```

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
# 使用 BFS 进行层序遍历

「BFS 遍历」、「层序遍历」、「最短路径」实际上是递进的关系。在 BFS 遍历的基础上区分遍历的每一层，就得到了层序遍历。在层序遍历的基础上记录层数，就得到了最短路径。

BFS 遍历是一类很值得反复体会和练习的题目。一方面，BFS 遍历是一个经典的基础算法，需要重点掌握。另一方面，我们需要能根据题意分析出题目是要求最短路径，知道是要做 BFS 遍历。

[LeetCode 102 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

```java

public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();

    Queue<TreeNode> queue = new ArrayDeque<>();
    if (root != null) {
        queue.add(root);
    }
    while (!queue.isEmpty()) {
        int n = queue.size();
        List<Integer> level = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            TreeNode node = queue.poll();
            level.add(node.val);
            if (node.left != null) {
                queue.add(node.left);
            }
            if (node.right != null) {
                queue.add(node.right);
            }
        }
        res.add(level);
    }

    return res;
}

```
# 使用 BFS 求解最短路径问题

在一棵树中，一个结点到另一个结点的路径是唯一的，但在图中，结点之间可能有多条路径，其中哪条路最近呢？这一类问题称为最短路径问题。最短路径问题也是 BFS 的典型应用，而且其方法与层序遍历关系密切。

在二叉树中，BFS 可以实现一层一层的遍历。在图中同样如此。从源点出发，BFS 首先遍历到第一层结点，到源点的距离为 1，然后遍历到第二层结点，到源点的距离为 2…… 可以看到，用 BFS 的话，距离源点更近的点会先被遍历到，这样就能找到到某个点的最短路径了。

[LeetCode 1162 离开陆地的最远距离](https://leetcode-cn.com/problems/as-far-from-land-as-possible/)

你现在手里有一份大小为 N x N 的 网格 grid，上面的每个 单元格 都用 0 和 1 标记好了。其中 0 代表海洋，1 代表陆地，请你找出一个海洋单元格，这个海洋单元格到离它最近的陆地单元格的距离是最大的。

我们这里说的距离是「曼哈顿距离」（ Manhattan Distance）：(x0, y0) 和 (x1, y1) 这两个单元格之间的距离是 |x0 - x1| + |y0 - y1| 。

如果网格上只有陆地或者海洋，请返回 -1。

示例 1：

输入：[[1,0,1],[0,0,0],[1,0,1]]
输出：2
解释：
海洋单元格 (1, 1) 和所有陆地单元格之间的距离都达到最大，最大距离为 2。
示例 2：

输入：[[1,0,0],[0,0,0],[0,0,0]]
输出：4
解释：
海洋单元格 (2, 2) 和所有陆地单元格之间的距离都达到最大，最大距离为 4。
 
提示：

1 <= grid.length == grid[0].length <= 100
grid[i][j] 不是 0 就是 1



# 参考
作者：nettee
链接：https://leetcode-cn.com/problems/binary-tree-level-order-traversal/solution/bfs-de-shi-yong-chang-jing-zong-jie-ceng-xu-bian-l/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/as-far-from-land-as-possible
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/maximum-depth-of-binary-tree
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

作者：sweetiee
链接：https://leetcode-cn.com/problems/as-far-from-land-as-possible/solution/jian-dan-java-miao-dong-tu-de-bfs-by-sweetiee/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。