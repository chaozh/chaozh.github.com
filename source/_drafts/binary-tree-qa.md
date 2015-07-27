title: 面试题总结：二叉树与链表问题
tags:
  - 算法
  - 面试
id: 728
categories:
  - 面试总结
date: 2013-09-17 16:15:37
---

二叉树

1\. 寻找一条路径等于给定值

方法1：如果有父节点链接，那么遍历每个节点回溯路径的和即可。

方法2：根据二叉树的先根遍历思想，通过一个栈保存从根到当前节点的路径，每遍历一个节点，都从sum值中减去此节点的权值，此点遍历结束后，再从栈中弹出此节点，并在sum中加上此节点的权值。当sum为零且当前节点为叶子节点时，打印栈中保存的路径。

2\. 给前序和中序序列还原二叉树

前序的第一个节点preOrder[0]即根节点，然后查找在中序中的位置nodeNumLeft，递归左树(preOrder+1, inOrder, nodeNumLeft)，递归右树(preOrder+1+nodeNumLeft, inOrder+nodeNumLeft+1, len-1-nodeNumLeft)

3\. 树中节点最大距离

递归如果树非空则要么是左子树中的最大距离，要么是右子树中的最大距离，要么是左子树到根距离与右子树到根距离之和。前面的两个递归即可算出后面所需的到根距离。

4\. 判断子树（大数据情况）

5\. 找最近的公共父节点

基本情况就分为在父节点两边或同一边
<pre>struct bt_tree* find_ancestor(struct bt_tree* head, struct bt_tree* n1, struct bt_tree *n2){
     if(head == NULL) return NULL;
     if(head == n1 || head == n2) return head; //check if ancestor
     struct bt_tree* left = find_ancestor(head-&gt;left, n1, n2);
     struct bt_tree* right = find_ancestor(head-&gt;right, n1, n2);
     if(left &amp;&amp; right) return head;
     return left?left:right;
}</pre>
6\. 判断是否为平衡树

使用后根遍历的思想进行比较，先递归下去，然后判断子树的高度差不大于1即可。

&nbsp;

链表

1\. 判断一个链表是否存在环

2\. 判断两个链表是否有公共节点

3\. 将链表反向

4\. 将链表按照一前一后的顺序重新排放

5\. 合并链表并去除重复的节点

6. 将链表排序

7\. 内核链表的各个宏定义