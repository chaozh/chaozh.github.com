title: Cracking the coding interview Q7-12
categories:
- 课程总结
tags:
  - 算法
  - 面试
id: 598
date: 2013-08-15 13:02:54
---

**7.7 Explain how you would design a chat server In particular, provide details about the various backend components, classes, and methods. What would be the hardest problems to solve?**

We can learn from twitter and QQ server design. The main concepts of the chat server would be user(status), chat group and messages.
<pre>enum Status{offline, online, away}
struct Message{ User from_user; User to_user; String post_message;}
class User {Status type; Message posts[]; sendMessage(); addGroup(); updateStatus(); createGroup(); } 
struct Group { Message posts[]; User list[];}</pre>
Information sync with memory cache and database; Server scale; User session management;

**7.9 Explain the data structures and algorithms that you would use to design an in-memory file system. Illustrate with an example in code where possible**

File system must inlude dir and file structure. The key points are design of super block, directory and inodes. Super block mainly deal with namespace and organize inodes, which we could use hash table for fast fetching. Inodes contains meta data and organize datas, which we could use B tree (memory efficient) or radix tree(linux, fast fetching) or log structure (journaling tech for availability). Directory mainly deal with file lookup operations which convert string path to the rightful inode.

**7.10 Describe the data structures and algorithms that you would use to implement a garbage collector in C++**

Organize object with Smart ptr using  reference counting tech(bad with dead lock). Will java's mark-sweep tech work?

**8.1 Write a method to generate the nth Fibonacci number**
<pre>def fibonacci(n):
	a,b=0,1
	while(n&gt;0):
		a,b,n=b,a+b,n-1
	return a</pre>
**8.2 Imagine a robot sitting on the upper left hand corner of an NxN grid. The robot can only move in two directions: right and down. How many possible paths are there for the robot? FOLLOW UP Imagine certain squares are “off limits”, such that the robot can not step on them. Design an algorithm to get all possible paths for the robot.**

Using DFS and traceback to print all paths.

**8.3 Write a method that returns all subsets of a set.**

** ** It's easy to solve this problem using 2^n bitmap for all subsets status and go thourgh to print corresponding subset.
<pre>def subsets(a, idx, len):
	sbs = []
	if idx == len:
		sb = []
		sbs.append(sb) #empty set
		print(sb)
	else:
		rsb = subsets(a, idx+1, len)
		e = a[idx]
		#print(idx,rsb)
		for i in rsb:
			t1 = i[:]
			sbs.append(t1) #keep origin
			t2 = i[:]
			t2.append(e) #add new elm
			sbs.append(t2)
			print(t2)
	return sbs</pre>
**8.4 Write a method to compute all permutations of a string. **
<pre>void permutations(const string &amp;s, vector &amp;v){
	if (s == ""){
		v.push_back(s);
	}else{
		string c = s.substr(0, 1); // substr(offset, len)
		vector vt;

		permutations(s.substr(1), vt); // get rest permutations
		for (std::vector::iterator i = vt.begin(); i != vt.end(); ++i)
		{
			v.push_back(*i);
			//insert in any space between the character to get permutation
			for (int j = 0; j &lt;= i-&gt;length(); ++j)
			{
				string st = *i; //copy one
				st.insert(j, c);
				v.push_back(st);
			}

		}
	}
}</pre>
if duplicate character is allowed which may result in duplicate answers then
<pre>void unique_permutations(const string &amp;s, set &amp;v){
	if (s == ""){
		v.insert(s);
	}else{
		for (int i = 0; i &lt; s.length(); ++i){
			string c = s.substr(i, 1); // substr(offset, len)
			string t = s; //copy one for erase
			set vt;
			unique_permutations(t.erase(i, 1), vt); // get rest permutations
			for (std::set::const_iterator i = vt.begin(); i != vt.end(); ++i)
			{
				v.insert(*i);
				v.insert(c + *i);
			}
		}
	}
}</pre>
**8.5 Implement an algorithm to print all valid (e g , properly opened and closed) combi -nations of n-pairs of parentheses EXAMPLE: input: 3 (e g , 3 pairs of parentheses) output: ()()(), ()(()), (())(), ((()))**
<pre>void paretheses(int n, int idx, int len, string res){
	int i, j;
	int remain = idx + 1;
	if(n == len){
		res += "(";
		for(j = remain; j &gt; 0; --j){
			res += ")";
		}
		cout&lt;&lt;res&lt;&lt;","; 	
        }else{ 		
                 res += "("; 		
                 for(i = remain; i &gt;= 0; --i){
			string tmp = res;

			for(j = i; j &gt; 0; --j){
				tmp += ")";
			}

			paretheses(n + 1, remain - i, len, tmp);

		}
	}
}</pre>
**8.6 Implement the “paint fill” function that one might see on many image editing programs. That is, given a screen (represented by a 2 dimensional array of Colors), a point, and a new color, fill in the surrounding area until you hit a border of that color’. **

Using recursive DFS or better using BFS way to search, no need to traceback so pretty easy.

**8.7 Given an infinite number of quarters (25 cents), dimes (10 cents), nickels (5 cents) and pennies (1 cent), write code to calculate the number of ways of representing n cents**

It could be solved by generating function, but a little bit complicate:
<pre>int cents[] = {1, 5, 10, 25};
int caculate(int n){
	int i, j, k, res;
	int *c1 = new int[n+1];
	int *c2 = new int[n+1];
	for(i=0; i&lt;=n; ++i){
		c1[i] = 1;  //firstly one cent
		c2[i] = 0;
	}

	//then 5, 10, 25 cents
	for(i=1; i&lt;=3; ++i){

		for(j=0; j&lt;=n; ++j){
			for(k=0; k+j&lt;=n; k+=cents[i]){
				c2[j+k] += c1[j];
			}
		}

		for(j=0; j&lt;=n; ++j){
			c1[j] = c2[j];
			c2[j] = 0;
		}
	}

	res = c1[n];

	delete[] c1;
	delete[] c2;

	return res;
}</pre>
or using a recursive method:
<pre>int caculate2(int sum, int c, int n){
	int ways = 0;
	int i;
	if(sum &lt;= n){ 		
           if(sum == n) return 1; 		
                for(i=3; i&gt;=0; --i){
	            if(c &gt;= cents[i])
			ways += caculate2(sum+cents[i], cents[i], n);
		}
	}
	return ways;
}</pre>
**9.2 Write a method to sort an array of strings so that all the anagrams are next to each other **

Simply sort every string alphabetically first and then compare with other strings (c++ string could use "&gt;" op to compare).

**9.3 Given a sorted array of n integers that has been rotated an unknown number of times, give an O(log n) algorithm that finds an element in the array.You may assume that the array was originally sorted in increasing order EXAMPLE: Input: find 5 in array (10 14 15 16 19 20 25 1 3 4 5 7) Output: 10 (the index of 5 in the array)**
<pre>int search(int n, int a[], int len){
    int low, high, mid;
    low = 0, high = len-1;
    while(low&lt;=high){
       mid = (low + high)/2;
       if(a[mid] == n){
          return mid;
       }else if (a[mid] &lt; n){ //25 1 3 4 5 7 10 14 15 16 19 20
           if(a[mid] &lt; a[low]){
               if(a[low] &lt; n) high = mid -1;                
               else low = mid + 1;            
           }else                
               low = mid + 1;        
       }else if (a[mid] &gt; n){
           if(a[mid] &lt; a[low]) high = mid -1;
           else{
              if(a[low] &lt; n) high = mid - 1;
              else low = mid + 1;
           }
       }
    } 
}</pre>
**9.4  If you have a 2 GB file with one string per line, which sorting algorithm would you use to sort the file and why?**

&nbsp;

**9.5 Given a sorted array of strings which is interspersed with empty strings, write a method to find the location of a given string
Example: find “ball” in [“at”, “”, “”, “”, “ball”, “”, “”, “car”, “”, “”, “dad”, “”, “”] will return 4
Example: find “ballcar” in [“at”, “”, “”, “”, “”, “ball”, “car”, “”, “”, “dad”, “”, “”] will return -1**

Using binary search method just moving right to neglect empty strings.

**9.6 Given a matrix in which each row and each column is sorted, write a method to find an element in it. **
Just search line by line in O(m+n) time:
<pre>int search_matrix(int mat[][], int m, int n, int x){
    int row = 0, col = n-1;
    while(row&lt;=m &amp;&amp; col &gt;=0){
        if(mat[row][col] == x) return row * n + col;
        else if(mat[row][col] &gt; x) --col;
        else  ++row;
    }
}</pre>
Binary search method also works:
<pre>int search_matrix(int mat[][], int m, int n, int x){
    int r1 = 0, c1 = 0;
    int r2 = m-1, c2 = n-1;
    int i = (r1+r2)/2, j = (c1+c2)/2;
    while((i!=r1||i!=r2) &amp;&amp; (j!=c1||j!=c2)){ //may have some problems
        if(mat[i][j] == x)
           return i*n+j;
        else if(mat[i][j] &lt; x){
            r2 = i, c2 = j;
        }else {
            r1 = i, c1 = j;
        }
        i = (r1+r2)/2, j = (c1+c2)/2;
    }
}</pre>
**9.7 A circus is designing a tower routine consisting of people standing atop one another’s shoulders. For practical and aesthetic reasons, each person must be both shorter and lighter than the person below him or her. Given the heights and weights of each person in the circus, write a method to compute the largest possible number of people in such a tower.
EXAMPLE:
Input (ht, wt): (65, 100) (70, 150) (56, 90) (75, 190) (60, 95) (68, 110)
Output:The longest tower is length 6 and includes from top to bottom: (56,90) (60,95) (65,100) (68,110) (70,150) (75,190)**

It's a LIS problem which can be solved in O(nlgn) by mento and binary search. (hdu1025)

**10.2 There are three ants on different vertices of a triangle. What is the probability of collision (between any two or all of them) if they start walking on the sides of the triangle? Similarly find the probability of collision with ‘n’ ants on an ‘n’ vertex polygon.**

Q1:For three ants and triangle, every ant has two directions and vertices to crawl, so there will be 2^3 conditions and only when all ants choose the same direction(clockwise or anticlockwise) will lead them not to collision. So probability of collision is 1 - 2/2^3

Q2: Similarly, probability of collision is 1 - 2/2^n

**10.3 Given two lines on a Cartesian plane, determine whether the two lines would intersect. **

line is different from segment, but the latter is more difficlut to judge if intersect with each other which could be solved by vector multiply.

**10.4 Write a method to implement *, - , / operations You should use only the + operator**

For multiply operation:
<pre>for i in range (1,n): 
         res+=m</pre>
For minus operation:
<pre>for i in range(0,n): 
     if (i + m == n): 
         return i</pre>
For division operation:
<pre>res=0
for i in range(1,m): 
    res+=n 
    if(res==m):
       return i
    if(res&gt;m):
       return i-1</pre>
**10.5 Given two squares on a two dimensional plane, find a line that would cut these two squares in half **

Any line go though the center of square would cut it in half, so a line go though both squares' center would be the answer. Just be careful with the case that both sqaures' center are the same point.

**10.6 Given a two dimensional graph with points on it, find a line which passes the most number of points**

Rather than using two points pair methond to represent the segment(we could also go though and check every pair in O(n^3) time), we could use slope and y-intercept thus this problem is no different than finding most common number in a list of numbers.

**10.7 Design an algorithm to find the kth number such that the only prime factors are 3, 5, and 7**

Using O(n^2) time to caculate one by one, which can be accelerated by hash list to reduce repeated aculation(hdu1058)

**11.2 You are given the source to an application which crashes when it is run. After running it ten times in a debugger, you find it never crashes in the same place. The application is single threaded, and uses only the C standard library. What programming errors could be causing this crash? How would you test each one?**

Memory leak causes out of memory and leads to crash or variable is not initialized with proper value. Unproper system configuration could also leads to crash (eg. max fd or max thread numbers) and sometimes interacting with hardware may also cause such problems.

**11.3 We have the following method used in a chess game: boolean canMoveTo(int x, int y) x and y are the coordinates of the chess board and it returns whether or not the piece can move to that position. Explain how you would test this method **

Invalid conditions for four corners and all possible move in board.
Valid conditions for all direction and all pieces.

**11.4 How would you load test a webpage without using any test tools?**

Load test usaually includes thoughput, response time, resource utilization and pressure tests.

**11.5 How would you test an ATM in a distributed banking system?**

Test should include UI test (accessiblity test) and functional test (single machine test and ditributed system test).

**12.1 If you were integrating a feed of end of day stock price information (open, high, low, and closing price) for 5,000 companies, how would you do it? You are responsible for the development, rollout and ongoing monitoring and maintenance of the feed. Describe the different methods you considered and why you would recommend your approach.The feed is delivered once per trading day in a comma separated format via an FTP site. The feed will be used by 1000 daily users in a web application.**

This problem actually ask you how to store this feed. SQL Database would be a good choice. NoSQL storage engine like mogodb would also do the job. Format could be XML or JSON.

**12.2 How would you design the data structures for a very large social network (Facebook, LinkedIn, etc)? Describe how you would design an algorithm to show the connection, or path, between two people (eg, Me-&gt;Bob-&gt;Susan-&gt;Jason-&gt;You)**

Using array to store friends' id so a graph in general. We could use other information to seperate people's information cleverly.

**12.3 Given an input file with four billion integers, provide an algorithm to generate an integer which is not contained in the file. Assume you have 1 GB of memory. FOLLOW UP What if you have only 10 MB of memory?**

If 1 GB memory we could simply use bitmap approach. For 10 MB memory we have to deal with blocks of numbers firstly using counting method and then deal with proper blocks using bitmap.

**12.4 You have an array with all the numbers from 1 to N, where N is at most 32,000\. The array may have duplicate entries and you do not know what N is. With only 4KB of memory available, how would you print all duplicate elements in the array?**

Just use bitmap method as we have space for 0 ~ 4*2^10*8 which is exactly bigger than 32,000.

**12.5 If you were designing a web crawler, how would you avoid getting into infinite loops?**

Simply try DFS or BFS approach to go through the whole gragh which means you have got a map to store the all urls as points.

**12.6 You have a billion urls, where each is a huge page. How do you detect the duplicate documents?**

You have to check the content of huge web page for url rather than url it self. To content comparing we could use hash finger method(or other similar method), then we've got a billion finger prints which is still a large number. We could hash those finger prints to different files and deal with every file in memory one by one or hash those to different machine.

**12.7 You have to design a database that can store terabytes of data. It should support efficient range queries. How would you do it?**

Using B+ tree to build the database.