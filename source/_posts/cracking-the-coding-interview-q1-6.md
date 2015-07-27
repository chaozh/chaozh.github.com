title: Cracking the coding interview Q1-6
tags:
  - 算法
  - 面试
id: 569
categories:
  - 面试总结
date: 2013-08-12 03:09:17
---

**1\. 1 Implement an algorithm to determine if a string has all unique characters. What if you can not use additional data structures?**

It's means to check if a string has repeated character or not. It can be solved by using a bitmap and Assic table(128 for normal and 256 with special). key: using `n=(int)string[i]` to get Assic number and `(bitmap[n/32] &amp; 1&lt;&lt;(n%32))` to check if repeated.

**1.2 Write code to reverse a C-Style String (C-String means that “abcd” is represented as five characters, including the null character )?**

Exchange characters between start index 0 and end index string length - 1, stop when two pointers meet.

**1.3 Design an algorithm and write code to remove the duplicate characters in a string without using any additional buffer. NOTE: One or two additional variables are fine. An extra copy of the array is not. FOLLOW UP Write the test cases for this method ?**

Not allowed to apply for new arrays. Compare each characters in the string and if duplicated just remove in place, key: using null character to replace duplicated  character. Test cases should include unique characters string, null string, string with only one character, common string with sequence  duplicate characters or with not.

**1.4 Write a method to decide if two strings are anagrams or not ?**

If new array is allowed to apply for, it can be solved by counting table in O(n) time. Actually sort two strings and compare each characters is enough to get the answer in O(nlgn) time.

**1.5 Write a method to replace all spaces in a string with ‘%20’ .**

Firstly count the ectra space needed by replacing, two chars' space for each character. If  space is enough then just fill and move, otherwise you should realloc space and copy whole string, don't forget the null character.

**1.6  Given an image represented by an NxN matrix, where each pixel in the image is 4 bytes, write a method to rotate the image by 90 degrees Can you do this in place ?**

To rotate 90 degrees in clockwise just exange M[i][j] with M[j][N-i], implementation could firstly exange elms between diagonal and then exange columns i with N-i

**1.7 Write an algorithm such that if an element in an MxN matrix is 0, its entire row and column is set to 0.**

Go through the matrix only to register  which column or row has zero and then go through the matrix again to set those rows or columns to zero which takes O(mn) time, key: only need array row[m] nd column[n]

**1.8 Assume you have a method isSubstring which checks if one word is a substring of  another. Given two strings, s1 and s2, write code to check if s2 is a rotation of s1 using only one call to isSubstring (i e , “waterbottle” is a rotation of “erbottlewat”)**

To use isSubstring function we need string s1 is longer than s2, so just duplicate s1 by s1' = s1 + s1 (s1' contains all rotations of s1) then check if s2 is substring of s1' to judge rotaion.

**2.1 Write code to remove duplicates from an unsorted linked list FOLLOW UP How would you solve this problem if a temporary buffer is not allowed?**

Using hash table could be the best way. If temporay buffer is not allowed then just using algorithm in 1.3 taking O(n^2) time.

**2.2 Implement an algorithm to find the nth to last element of a singly linked list.**

Using two pointers at a distance of n to solve. Another way is using recursion as stack.

**2.3 Implement an algorithm to delete a node in the middle of a single linked list, given only access to that node**

**EXAMPLE**
** Input: the node ‘c’ from the linked list a-&gt;b-&gt;c-&gt;d-&gt;e**
** Result: nothing is returned, but the new linked list looks like a-&gt;b-&gt;d-&gt;e**

Just copy the data from next node and delete next node by pointing the next pointer field to the node after next node. Note that there would be a problem if the node is the last node in the list.

**2.4 You have two numbers represented by a linked list, where each node contains a single digit The digits are stored in reverse order, such that the 1’s digit is at the head of the list. Write a function that adds the two numbers and returns the sum as a linked list**

**EXAMPLE**
** Input: (3 -&gt; 1 -&gt; 5) + (5 -&gt; 9 -&gt; 2)**
** Output: 8 -&gt; 0 -&gt; 8**

Just pay attention to null list and list in different length.

**3.1 Describe how you could use a single array to implement three stacks**

It's easy to implement with three stack top pointers to seperate space.

**3.2 How would you design a stack which, in addition to push and pop, also has a function min which returns the minimum element? Push, pop and min should all operate in O(1) time**

It's hard to deal with the pop when  min element was on the top of the stack. So extra space are needed to store those bigger elms. An extra stack could be used to store min elms. Every time you push a new elem, compare it with the top of extra stack and smaller one has to be pushed into extra stack. The same, when pop out a elem you should check if it equal to the top of extra stack and if so you have to pop it out from the extra stack.

**3.3 Imagine a (literal) stack of plates. If the stack gets too high, it might topple. Therefore, in real life, we would likely start a new stack when the previous stack exceeds some threshold. Implement a data structure SetOfStacks that mimics this. SetOfStacks should be composed of several stacks, and should create a new stack once the previous one exceeds capacity. SetOfStacks push() and SetOfStacks pop() should behave identically to a single stack (that is, pop() should return the same values as it would if there were just a single stack) FOLLOW UP Implement a function popAt(int index) which performs a pop operation on a specific sub-stack**

It's worth implementing in C++

**3.4 In the classic problem of the Towers of Hanoi, you have 3 rods and N disks of different sizes which can slide onto any tower The puzzle starts with disks sorted in ascending order of size from top to bottom (e g , each disk sits on top of an even larger one) You have the following constraints:**
** (A) Only one disk can be moved at a time**
** (B) A disk is slid off the top of one rod onto the next rod**
** (C) A disk can only be placed on top of a larger disk**
** Write a program to move the disks from the first rod to the last using Stacks**

    void hanoi(int n, char src, char bri, char dst){
        if(n==1){
            cout&lt;&lt;"Move disk "&lt;&lt;n&lt;&lt;" from "&lt;&lt;src&lt;&lt;" to "&lt;&lt;dst&lt;&lt;endl;
        }
        else{
            hanoi(n-1, src, dst, bri);
            cout&lt;&lt;"Move disk "&lt;&lt;n&lt;&lt;" from "&lt;&lt;src&lt;&lt;" to "&lt;&lt;dst&lt;&lt;endl;
            hanoi(n-1, bri, src, dst);
        }
    }
    `</pre>
    You should change it with using stack

    **3.5 Write a program to sort a stack in ascending order. You should not make any assumptions about how the stack is implemented. The following are the only functions that should be used to write this program: push | pop | peek | isEmpty.**

    If only stack could be used then an extra stack is needed to simulate insert sort

    **4.1 Implement a function to check if a tree is balanced. For the purposes of this question, a balanced tree is defined to be a tree such that no two leaf nodes differ in distance from the root by more than one**

    Just get every leaf node's depth by tree walk(time O(n)) and store them (space O(n/2)) then go through the array to judge if the tree is unbalanced or check the tree in a postorder that is recursively down and then judge.

    **4.2 Given a directed graph, design an algorithm to find out whether there is a route between two nodes**

    Using BFS to find.

    **4.3 Given a sorted (increasing order) array, write an algorithm to create a binary tree with minimal height **

    It's easy to use recursion way to build the tree with limited length. You can use binary search way to build binary search tree.

    **4.4 Given a binary search tree, design an algorithm which creates a linked list of all the nodes at each depth (ie , if you have a tree with depth D, you’ll have D linked lists) **

    Just walk though the tree and link those nodes to the linked lists based on the depth of the nodes. A better solution is using BFS way to walk though the tree.
    <pre>`void tree_level_walk(struct bt_tree* root, vector&lt;list&gt; &amp;res){
    	list ltmp;
    	list::iterator it;
    	struct bt_tree* tmp;
    	int depth = 1;

    	ltmp.push_back(root);
    	res[depth].push_back(ltmp);

    	while(!res[depth].empty()){ // check every level
    		ltmp.clear();
    		for(it=res[depth].begin(); it!=res[depth].end(); ++it){ // using list as queue to store all next level nodes
    			tmp = *it;
    			if(tmp-&gt;left) ltmp.push_back(tmp-&gt;left);
    			if(tmp-&gt;right) ltmp.push_back(tmp-&gt;right);
    		}
    		++depth;
    		res[depth].push_back(ltmp);
    	}
    }
    `</pre>
    **4.5 Write an algorithm to find the ‘next’ node (ie , in-order successor) of a given node in a binary search tree where each node has a link to its parent**
    <pre>`def tree_successor(self, node):
         if node.right != None:
              return tree_min(node.right)
         tmp = node.parent
         while tmp != None and node == tmp.right:
              node = tmp
              tmp = node.parent
         return tmp
    `</pre>
    **4.6 Design an algorithm and write code to find the first common ancestor of two nodes in a binary tree. Avoid storing additional nodes in a data structure. NOTE: This is not necessarily a binary search tree **

    If you want store all ancestor of one node to check then using hash table instead of simple array. But it requires every node has a link to its parent and additional space. Else you can start from root and using recursive way to find result and take over very time you find a closer one.
    <pre>`bool father(struct bt_tree* n1, struct bt_tree* n2){
        if(n1 == NULL) return false;
        else if(n1 == n2) return true;
        else return father(n1-&gt;lchild, n2) || father(n1-&gt;rchild, n2);
    }

    void find_ancestor(struct bt_tree* head, struct bt_tree* n1, struct bt_tree* n2, struct bt_tree** res){
        if(head == NULL) return;
        if(head &amp;&amp; father(head, n1) &amp;&amp; father(head, n2)){
            res = head; 
            find_ancestor(head-&gt;left, n1, n2, res);
            find_ancestor(head-&gt;right, n1, n2, res);
        }
    }`</pre>
    Here is a better solution to check less node
    <pre>`struct bt_tree* find_ancestor(struct bt_tree* head, struct bt_tree* n1, struct bt_tree *n2){
         if(head == NULL) return NULL;
         if(head == n1 || head == n2) return head; //check if ancestor
         struct bt_tree* left = find_ancestor(head-&gt;left, n1, n2);
         struct bt_tree* right = find_ancestor(head-&gt;right, n1, n2);
         if(left &amp;&amp; right) return head;
         return left?left:right;
    }`</pre>
    **4.7 You have two very large binary trees: T1, with millions of nodes, and T2, with hundreds of nodes. Create an algorithm to decide if T2 is a subtree of T1**

    Using BFS way to go though T1 and compare with T2 if the node is equal with T2's root. Using string comparing technique is a better way for it's easier to store in disk file so just compare inorder and preoder sequence. It's a really hard to deal with such big data.

    **4.8 You are given a binary tree in which each node contains a value. Design an algorithm to print all paths which sum up to that value. Note that it can be any path in the tree it does not have to start at the root. **

    You can walk though the tree and backtrace every node if it has a link to parent to calculate the sum and judge.

    **5.1 You are given two 32-bit numbers, N and M, and two bit positions, i and j. Write a method to set all bits between i and j in N equal to M (eg, M becomes a substring of N located at i and starting at j)
    EXAMPLE:
    Input: N = 10000000000, M = 10101, i = 2, j = 6
    Output: N = 10001010100**

    (N ^ ~(1&lt;&lt;(j-i+1)-1)&lt;&lt;i) | (M&lt;&lt;i)

    **5.2 Given a (decimal - eg 3.72) number that is passed in as a string, print the binary representation. If the number can not be represented accurately in binary, print “ERROR”**

    Using '%' op to deal with integer part and  multiply op to deal with decimal part which should be finished the same time s as its length, for example  decimal part of 3.72  should be finished in two times of multiply or should print "ERROR"

    **5.3 Given an integer, print the next smallest and next largest number that have the same number of 1 bits in their binary representation**

    To make a larger number just find the 1st zero bit after 1 bits and change that bit into 1 and set all lower bit into 0, in the mean time of going though the number remember to count the number of 1 bit (not including the last 1 bit before the changed bit) and then set the same number of 1 bit from the lowest bit place. A better way to count 1 bit is using following method:
    <pre>`
    int count_one(int x){
        x = (x &amp; (0x55555555)) + ((x &gt;&gt; 1) &amp; (0x55555555));
        x = (x &amp; (0x33333333)) + ((x &gt;&gt; 2) &amp; (0x33333333));
        x = (x &amp; (0x0f0f0f0f)) + ((x &gt;&gt; 4) &amp; (0x0f0f0f0f));
        x = (x &amp; (0x00ff00ff)) + ((x &gt;&gt; 8) &amp; (0x00ff00ff));
        x = (x &amp; (0x0000ffff)) + ((x &gt;&gt; 16) &amp; (0x0000ffff));
        return x;
    }

To make a lower number is the same just find the 1st 1 bit after zero bits and count 1 bits(including the changed bit) and then set.
Yet you should be careful with the negative numbers.

**5.4 Explain what the following code does: ((n &amp; (n-1)) == 0)**

To check if n is power of 2.

**5.5 Write a function to determine the number of bits required to convert integer A to integer B
Input: 31, 14
Output: 2**

Using (1&lt;&lt;i) and operation and to go though two number and count how many bit are different. A better way is to count the 1 in result of A ^ B.

**5.6 Write a program to swap odd and even bits in an integer with as few instructions as possible (eg, bit 0 and bit 1 are swapped, bit 2 and bit 3 are swapped, etc)**

((x &amp; 0xaaaaaaaa) &gt;&gt; 1) | ((x &amp; 0x55555555) &lt;&lt; 1)

**5.7 An array A[1...n] contains all the integers from 0 to n except for one number which is missing. In this problem, we cannot access an entire integer in A with a single operation. The elements of A are represented in binary, and the only operation we can use to access them is “fetch the jth bit of A[i]”, which takes constant time. Write code to find the missing integer. Can you do it in O(n) time?**

Testing number of every bit in the array to select the minimal part and make the missing element.
<pre>def find_missing(a[], n):
     len = n-1
     column = lg(n)
     res = 0
     for j in range(column-1, 0, -1):
          count0=count1=0
          a1=a2=[]
          for i in a: 
              if (getBit(i, j)):
                  a1[count1++]=i
              else:
                  a0[count0++]=ai
          if count1 &gt;= count0: #may have problem when count eqs to zero
              a=a0, len = count0
          else: 
              a=a1, len = count1
              res+=(1&lt;&lt;j)
     return res</pre>
Remember missing one number problem can always be solved by sumup(1,n)-sumup(a[1],a[n])

**6.1 Add arithmetic operators (plus, minus, times, divide) to make the following expression true: 3 1 3 6 = 8   You can use any parentheses you’d like**

(3+1)/3*6=8

**6.2 There is an 8x8 chess board in which two diagonally opposite corners have been cut off You are given 31 dominos(多米诺骨牌), and a single domino can cover exactly two squares Can you use the 31 dominos to cover the entire board? Prove your answer (by providing an example, or showing why it’s impossible)**

As two corners painted in the same color are cut off. It's imporssible to cover with uncoupled numbers of squares.

**6.3 You have a five quart jug and a three quart jug, and an unlimited supply of water (but no measuring cups) How would you come up with exactly four quarts of water? NOTE: The jugs are oddly shaped, such that filling up exactly ‘half’ of the jug would be impossible**

Firstly using full five quart jug minus three quart jug(5-3=2), then put the rest in three quart jug(3-2=1) and then use full five quart jug to fill three quart jug(5-1=4).

**6.4 A bunch of men are on an island A genie comes down and gathers everyone together and places a magical hat on some people’s heads (ie , at least one person has a hat) The hat is magical: it can be seen by other people, but not by the wearer of the hat himself. To remove the hat, those (and only those who have a hat) must dunk themselves underwater at exactly midnight If there are n people and c hats, how long does it take the men to remove the hats? The men cannot tell each other (in any way) that they have a hat.**

All men on the island knowns there exists at least one hat. So one hat takes one day, two hats take two days and c hats take c days to remove.

**6.5 There is a building of 100 floors If an egg drops from the Nth floor or above it will break If it’s dropped from any floor below, it will not break You’re given 2 eggs Find N, while minimizing the number of drops for the worst case.**

Using section to reduce number of drops, `x + (x-1) + (x-2) + ... + 1 &gt;= 100` and x eqs to 14 then sections will be `14, 27, 39, 50, 60, 69, 77, 84, 90, 95, 99, 100` and then first egg to test the section and second egg to test level sequencely.

**6.6 There are one hundred closed lockers in a hallway. A man begins by opening all one hundred lockers. Next, he closes every second locker. Then he goes to every third locker and closes it if it is open or opens it if it is closed (eg , he toggles every third locker). After his one hundredth pass in the hallway, in which he toggles only locker number one hundred, how many lockers are open?**

Except 1 and the number itself, if the number i can be divided exactly by factor a then it can be diveded exactly by factor i/a meaning that number of factors would be even. Only when i=i/a the number of factors will become odd and the number i must be the full square so from 1 to 100 will be 10 lockers remain open.