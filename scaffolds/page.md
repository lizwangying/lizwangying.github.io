---
title: {{ title }}
date: {{ date }}
---  

#二叉树    
</br>
##二叉树遍历分为：
* 二叉树深度优先遍历
	* 前序遍历
	* 中序遍历
	* 后序遍历
* 二叉树广度优先遍历
	* 层序优先遍历  


####记忆心得： 
前中后是将根节点作为参照物，比如前序就是根节点第一个遍历，中序就是根节点第二个，后序就是根节点最后。

后文以此二叉树为例： 

		 F
		/  \
	   B    G
	  /\     \
	 A  D     I
	    /\   /
	   C E  H
	   

</br>
## 前序遍历
前序遍历首先访问根节点，然后遍历左子树，最后遍历右子树。  
结果：F B A D C E G I H 

## 中序遍历
中序遍历是先遍历左子树，然后访问根节点，然后遍历右子树。  
结果：A B C D E F G **H I**  

通常来说，对于二叉搜索树，我们可以通过中序遍历得到一个递增的有序序列。 我们将在另一张卡片（数据结构介绍 – 二叉搜索树）中再次提及。

## 后序遍历
后序遍历是先遍历左子树，然后遍历右子树，最后访问树的根节点。  
结果：A C E D B H I G F  

值得注意的是，当你删除树中的节点时，删除过程将按照后序遍历的顺序进行。 也就是说，当你删除一个节点时，你将首先删除它的左节点和它的右边的节点，然后再删除节点本身。  

另外，后序在数学表达中被广泛使用。 编写程序来解析后缀表示法更为容易。 这里是一个例子：

		+
	   / \
	  *   5
	 /\
	4  -
	  / \
	 7   2
	   
中序遍历：4 * 7 - 2 + 5  
后序遍历：4 7 2 - * 5 +

您可以使用**中序遍历轻松找出原始表达式**。 但是程序处理这个表达式时并不容易，因为你必须检查操作的优先级。

如果你想对这棵树进行后序遍历，使用栈来处理表达式会变得更加容易。 每遇到一个操作符，就可以从栈中弹出栈顶的两个元素，计算并将结果返回到栈中。

<br>
#递归遍历
<br>
####例题：
输入: [1,null,2,3]    

	1
    \
     2
    /
    3   

输出: [1,2,3]  
####解题代码：
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 * --------------------
 * coding 中遇到的问题：
 * 1. 递归过程中 new 对象感觉很浪费资源，并且会被覆盖啊，怎么创建对象呢？  
 * 不能重复创建对象，尤其是返回值，所以需要将递归的部分新建一个函数，
 * 创建对象放在外面，返回值作为参数传递给递归函数，新建对象放在调用递归之前。
 * 2. 内循环开始 return 的时机是什么？
 * 常见是当前元素判断为空。
 * 3. 整个递归的结果怎么 return?
 * 递归函数的参数记录最终的返回值。
 */
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList();
        traversal(root,result);
        return result;
    }

    public void traversal(TreeNode root , List<Integer> result){
        if(root != null){
            result.add(root.val);
            if(root.left != null){
                traversal(root.left,result);
            }
            if(root.right != null){
                traversal(root.right,result);
            }
        }else{
            return;
        }

    }
}
```
##递归要素
###1. 确定递归函数的参数和返回值：
确定哪些参数是递归的过程中需要处理的，那么就在**递归函数里加上这个参数**， 并且还要明确每次递归的返回值是什么进而确定递归函数的返回类型。

###2.确定终止条件：
写完了递归算法, 运行的时候，经常会遇到栈溢出的错误，就是没写终止条件或者终止条件写的不对，操作系统也是用一个栈的结构来保存每一层递归的信息，如果递归没有终止，操作系统的内存栈必然就会溢出。

###3.确定单层递归的逻辑：
确定每一层递归需要处理的信息。在这里也就会重复调用自己来实现递归的过程。

<br>
# 迭代遍历
<br>
迭代法一般都使用栈来实现。栈是先入后出，所以入栈顺序需要注意。


<br>
# 层序遍历
<br>

层序遍历就是逐层遍历树结构。通常，我们使用一个叫做队列（Queue）的数据结构来帮助我们做广度优先搜索。  
结果：F B G A D I C E H   

[例题](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

BFS使用队列，把每个还没有搜索到的点依次放入队列，然后再弹出队列的头部元素当做当前遍历点。BFS总共有两个模板：

如果不需要确定当前遍历到了哪一层，BFS模板如下。


``` 
while queue 不空：
    cur = queue.pop()
    for 节点 in cur的所有相邻节点：
        if 该节点有效且未访问过：
            queue.push(该节点)
```   

如果要确定当前遍历到了哪一层，BFS模板如下。
这里增加了level表示当前遍历到二叉树中的哪一层了，也可以理解为在一个图中，现在已经走了多少步了。size表示在当前遍历层有多少个元素，也就是队列中的元素数，我们把这些元素一次性遍历完，即把当前层的所有元素都向外走了一步。

```
level = 0
while queue 不空：
    size = queue.size()
    while (size --) {
        cur = queue.pop()
        for 节点 in cur的所有相邻节点：
            if 该节点有效且未被访问过：
                queue.push(该节点)
    }
    level ++;
```


上面两个是通用模板，在任何题目中都可以用，是要记住的！

本题要求二叉树的层次遍历，所以同一层的节点应该放在一起，故使用模板二。

使用队列保存每层的所有节点，每次把队列里的原先所有节点进行出队列操作，再把每个元素的非空左右子节点进入队列。因此即可得到每层的遍历。


<br>
#DFS 与 BFS 的特点比较
<br>
###BFS 的适用场景
###如何用 BFS 进行层序遍历
###如何用 BFS 求解最短路径问题


<br>
#运用递归解决树的问题
<br>

* [给定一个二叉树，请寻找它的最大深度。  ](https://leetcode-cn.com/leetbook/read/data-structure-binary-tree/xefb4e/) 
* [判断二叉树是否对称](https://leetcode-cn.com/problems/symmetric-tree/)
	* 递归解法： 双指针判断
	* 迭代解法：层序遍历，根节点入队列两次，然后每次拉出来两个节点对比是否相等 

##前序遍历（自顶向下）
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

##后序遍历（自底向上）  
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
##总结
了解递归并利用递归解决问题并不容易。

当遇到树问题时，请先思考一下两个问题：

你能确定一些参数，从该节点自身解决出发寻找答案吗？
你可以使用这些参数和节点本身的值来决定什么应该是传递给它子节点的参数吗？
如果答案都是肯定的，那么请尝试使用 “自顶向下” 的递归来解决此问题。

或者你可以这样思考：对于树中的任意一个节点，如果你知道它子节点的答案，你能计算出该节点的答案吗？ 如果答案是肯定的，那么 “自底向上” 的递归可能是一个不错的解决方法。

在接下来的章节中，我们将提供几个经典例题，以帮助你更好地理解树的结构和递归。



<br>
#题外话：
<br>
下面是Java中Queue的一些常用方法：    

|方法名     |作用      |异常处理     |
|---|---|---|
|add         |增加一个元索                      |如果队列已满，则抛出一个IIIegaISlabEepeplian异常|
|remove      |移除并返回队列头部的元素            |如果队列为空，则抛出一个NoSuchElementException异常
|element     |返回队列头部的元素                 |如果队列为空，则抛出一个NoSuchElementException异常
|offer       |添加一个元素并返回true             |如果队列已满，则返回false
|poll        |移除并返问队列头部的元素            |如果队列为空，则返回null
|peek        |返回队列头部的元素                 |如果队列为空，则返回null
|put         |添加一个元素                      |如果队列满，则阻塞
|take        |移除并返回队列头部的元素    



#参考：  
作者：fuxuemingzhu
链接：https://leetcode-cn.com/problems/binary-tree-level-order-traversal/solution/tao-mo-ban-bfs-he-dfs-du-ke-yi-jie-jue-by-fuxuemin/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。   

作者：力扣 (LeetCode)
链接：https://leetcode-cn.com/leetbook/read/data-structure-binary-tree/xefb4e/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
