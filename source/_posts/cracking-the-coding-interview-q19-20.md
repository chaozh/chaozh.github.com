title: Cracking the coding interview Q19-20
categories:
- 课程总结
tags:
  - 算法
  - 面试
id: 678
date: 2013-09-05 10:57:55
---

19.1 Write a function to swap a number in place without temporary variables
<pre>x=x^y y=x^y x=x^y</pre>
**19.2 Design an algorithm to figure out if someone has won in a game of tic-tac-toe**

We could use 1 for player1 and 2 for player2 and sum caculation to judge if any player wins or we can simply go though all directions to judge after any player make a move.

**19.3 Write an algorithm which computes the number of trailing zeros in n factorial**

Just caculate how many 5 are there in factors as 2 is usualy enough and be careful that 25 has two 5 rather than one.

<pre>
int NumZeros(int n){
    if(n < 0) return -1;
    int num = 0;
    while((n /= 5) > 0){ //deal with factor like 25
        num += n;
    }
    return num;
}
</pre>

**19.4 Write a method which finds the maximum of two numbers. You should not use if-else or any other comparison operator. EXAMPLE Input: 5, 10 Output: 10**

<pre>
int mmax(int a, int b){
    int c = a - b;
    int k = (c>>31) & 0x01; //sign bit
    return a - k*c; //marvelous
}
</pre>

**19.5 The Game of Master Mind is played as follows: 
The computer has four slots containing balls that are red (R), yellow (Y), green (G) or blue (B). For example, the computer might have RGGB (e.g., Slot #1 is red, Slots #2 and #3 are green, Slot #4 is blue)  
You, the user, are trying to guess the solution. You might, for example, guess YRGB When you guess the  correct color for the correct slot, you get a “hit”. If you guess a color that exists but is in the wrong  slot,  you get a “pseudo-hit”. For example, the guess YRGB has 2 hits and one pseudo hit. For each guess, you are told the number of hits and pseudo-hits Write a method that, given a guess and a solution, returns  the number of hits and pseudo hits. **

The key is to figure out if any color could be judge multiple times. If not, which is a harder problem, go though the guess and the solution in the same time and judge hit first then increase corresponding color in solution's count if not hit and reduce color in guess's count, remember if count is ever bigger or equal to zero then that leads to a pseudo hit count.

**19.6 Given an integer between 0 and 999,999, print an English phrase that describes the integer (eg, “One Thousand, Two Hundred and Thirty Four”)  **

Firstly check zero and then check thousand using length and be careful with eleven to nineteen. 

**19.7 You are given an array of integers (both positive and negative). Find the continuous sequence with the largest sum. Return the sum EXAMPLE Input: {2, -8, 3, -2, 4, -10} Output: 5 (i.e. , {3, -2, 4} ) **

It's a classic dp problem and dp[i] = max{dp[i-1]+k[i], k[i]}

**19.8 Design a method to find the frequency of occurrences of any given word in a book**

Using tire tree (or hash) and count every word.

**19.9 Since XML is very verbose, you are given a way of encoding it where each tag gets mapped to a pre-defined integer value. The language/grammar is as follows:  
  Element --> Element Attr* END Element END [aka, encode the element tag, then its attributes, then tack on an END character, then encode its children, then another end tag]
  Attr --> Tag Value [assume all values are strings]
  END --> 01
  Tag --> some predefined mapping to int
  Value --> string value END
Write code to print the encoded version of an xml element (passed in as string) 
FOLLOW UP
Is there anything else you could do to (in many cases) compress this even further?**

**19.10 Write a method to generate a random number between 1 and 7, given a method that generates a random number between 1 and 5(i.e., implement rand7() using rand5()). FOLLOW use randAB() to generate randCD()**

<pre>def rand7():
         ret = rand5()+rand5()
         if ret > 7:
            return rand7()
         return ret
     # A is bigger than B
     def randB():
         ret = randA()
         if(ret > B*(A/B))
            return randB()
         return ret%B+1
</pre>

**19.11 Design an algorithm to find all pairs of integers within an array which sum to a specified value.** 

Firstly sort and then go though the array from the lowest and highest to judge if their sum equals to a specified value.

**20.1 Write a function that adds two numbers. You should not use + or any arithmetic operators.**

<pre>
int add(int a, int b){
    while(b != 0){
        int sum = a^b; // (0,1)->1 (1,1)->0 (0,0)->0
        int carry = (a & b)<<1; // and and shift left to generate carry
        a = sum;
        b = carry;
    }
    return a;
}

// a better solution
int add(int a, int b){
    char *c = (char *)a; // convert int to address
    return (int)&c[b]; // using array address to add
}

</pre>

**20.2 Write a method to shuffle a deck of cards. It must be a perfect shuffle in other words, each 52! permutations of the deck has to be equally likely. Assume that you are given a random number generator which is perfect.**  

Randomly pick up a card from total and remove it in the next turn, code like build heap.

<pre>
def shuffle(cards):
    for i in range(0, len(cards)):
        j= random() % (len(cards)-i) + i #key step
        cards[i],cards[j]=cards[j],cards[i]
    return cards
</pre>

**20.3 Write a method to randomly generate a set of m integers from an array of size n. Each element must have equal probability of being chosen**

almost the same as the upper question.

**20.4 Write a method to count the number of 2s between 0 and n.**

check every factor and count by caculation.

**20.5 You have a large text file containing words. Given any two words, find the shortest distance (in terms of number of words) between them in the file. Can you make the searching operation in O(1) time? What about the space complexity for your solution?**

It must be solved by using hash table, yet it should be build by traverse the file and record every two words' min distance and hash key is based on the sum of words and takes O(n^2) time and space to build that hash table. We assume that word order does not matter.

**20.6 Describe an algorithm to find the largest 1 million numbers in 1 billion numbers. Assume that the computer memory can hold all one billion numbers**

It's easy to do with the min-heap and replacement algorithm (STL partial sort) or use O(n) partition algorithm (kth biggest elem) as all datas can be hold in memory.

**20.7 Write a program to find the longest word made of other words in a list of words EXAMPLE Input: test, tester, testertest, testing, testingtester Output: testingtester**

Sort the words from long to short by length firstly and check every word's prefix and rest letters if they appears in the words array and we could use hash table to optimize the check so the whole takes O(nlgn)+O(n*1*d) time.

**20.8 Given a string s and an array of smaller strings T, design a method to search s for each small string in T**

Using tire tree yet you must know how to build one. We can build one based on suffix of the string s (taking O(m^2) time and space to build the tree and judge in k*O(n) time) or better we can construct an AC automachine（taking O(m) time to build and k*O(n) + O(s) time to judge）

**20.9 Numbers are randomly generated and passed to a method. Write a program to find and maintain the median value as new values are generated **

We can build max-heap on the first half and min-heap on the second half then it takes O(1) to get median and O(nlgn) to maintain. In fact, we only need four（or five） numbers (min and max in two halfs) to get median and maintain.

**20.10 Given two words of equal length that are in a dictionary, write a method to transform one word into another word by changing only one letter at a time. The new word you get in each step must be in the dictionary EXAMPLE Input: DAMP, LIKE Output: DAMP -> LAMP -> LIMP -> LIME -> LIKE**

It could be solved by BFS and trace back method.

**20.11 Imagine you have a square matrix, where each cell is filled with either black or white. Design an algorithm to find the maximum subsquare such that all four borders are filled with black pixels**

It ask to find the biggest subsquare in black, just go though the square matrix and check. Remember we are deal with the square thus every line length should be equal so if col is smaller than current max then it's impossible to find a bigger subsquare and we can stop traverse.

**20.12 Given an NxN matrix of positive and negative integers, write code to find the submatrix with the largest possible sum**  

We can combine row i to j and form a new one-dimension array. It takes O(n) time to find largest sum in a one-dimension array and we get n^2 that kind of arrays in all, thus takes O(n^3) time as a whole.

**20.13 Given a dictionary of millions of words, give an algorithm to find the largest possible rectangle of letters such that every row forms a word (reading left to right) and every column forms a word (reading top to bottom)**

It ask to form a largest matrix of words so all the chosen words must be in the same length and we could use tire tree to judge prefix of the colums' words ever exists.