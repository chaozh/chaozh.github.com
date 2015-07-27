title: Cracking the coding interview Q13-18
tags:
  - 算法
  - 面试
id: 628
categories:
  - 面试总结
date: 2013-08-23 11:18:07
---

**13.1 Write a method to print the last K lines of an input file using C++.**
<pre>int tail(const string &amp;fname, int lines, list &amp;res)
{
    ifstream fs(fname.c_str());
    if(!fs)
        return -1;
    int status = 0;
    int bytes_perline = 100;
    fs.seekg(0, fs.end);//fs.end
    streamoff end_pos = fs.tellg();
    streamoff start_pos = end_pos - bytes_perline*lines;
    if(start_pos &lt; 0) start_pos = 0;
    fs.seekg(start_pos, fs.beg);

    string line;
    list::iterator it = res.begin();
    while(res.size() &lt; lines){         
       getline(fs, line);         
       it = res.insert(it, line);         
       ++it;         
       if(status == 0){ //find last lines             
         if(fs.eof()){                 
                if(res.size() &gt; lines)
                    break;
                end_pos = start_pos;
                start_pos&gt;&gt;=1;
                fs.clear();
                fs.seekg(start_pos, fs.beg);
                res.erase(--it);
                it=res.begin();
                status = 1;
            }
        }else if(status &gt; 0){ // look up in the front
            if(fs.tellg() &gt;= end_pos){
                if(res.size() &gt; lines)
                    break;
                end_pos = start_pos;
                start_pos&gt;&gt;=1;
                fs.clear();
                fs.seekg(start_pos, fs.beg);
                res.erase(--it);
                it=res.begin();
                 if(start_pos &lt;= 0) // reach the top                     
                    break;             
             }                  
        }    
   }         
   if(res.size()&gt; lines ){
       int i = res.size()-lines;
       for(;i&gt;0;--i)
          res.erase(res.begin());
   }

   return res.size();
}

int main(int argc, char const *argv[])
{
    int ret;
    list res;
    ret = tail(argv[1], atoi(argv[2]), res);
    if(ret&gt;0)
        copy(res.begin(), res.end(), ostream_iterator(cout, "\n"));
    return 0;
}</pre>
**13.2 Compare and contrast a hash table vs an STL map. How is a hash table implemented?  If the number of inputs is small, what data structure options can be used instead of  a hash table.**

STL map's pairs are in sorted order but hash table don't and STL map takes O(nlgn) to find a elm yet hash table takes O(1) time. When implement a hash table you should find a hash function, deal with collision and size of table. If inputs is small then RB tree would be a a alternative way for hash table.

**13.3  How do virtual functions work in C++?**

A virtual function depends on a "virtual table". If any function or class is declared as virtual, a v-table is constructed  which stores addresses of the virtual functions of this class. The compiler also adds a hidden vptr in all such classes which points to the vtable. If a virtual function is not overriden in the derived class, the vtable of the derived class stores the adress of the function in his parent class. The vtable is used to resolve the address of the function. Dynamic binding in C++ is therefore performed though the vtable mechanism.

**13.4 What is the difference between deep copy and shallow copy? Explain how you would use each.**

Shallow copy just copy the address of the object's ptr field yet deep copy also copy the values of that ptr points to which need dynamic allocated space.  The default copy constructor and assignment operator make shallow copies. Deep copy usually are used to deal with the string or buffer.

**13.5  What is the significance of the keyword “volatile” in C**

Volatile informs the compiler that the value of the variable can change from the outside, without any update done by the code, thus just don't optimize the variable related code and fetch the variable value from memory instead of the register.

**13.6  What is name hiding in C++? **

"Name hiding is particularly tricky if the derived class's method has a different signature than the base class's method of the same name." Base class's method would be hidden if the derived class overrides that method (no-virtual function) and overloaded in the same time. (see Effective C++ rule 33)

**13.7  Why does a  destructor in base class need to be declared virtual?**

If we delete on the base ptr which points to the derived class object, the base class destructor called first (for no-virtual function) which would lead to a memory leak for the derived part. We should declare base class's destructor in virtual so that derived destructor would be called first and destroy all parts of the object as we expected.

**13.8 Write a method that takes a pointer to a Node structure as a parameter and returns a complete copy of the passed-in data structure.The Node structure contains two pointers to other Node structures.**

It's mush alike deep copy a binary tree.
<pre>typedef struct Node Node_t;
struct Node{
   struct Node* ptr1;
   struct Node* ptr2; 
};
typedef set&lt;Node_t*&gt; NodeMap;

Node_t* copy_recursive(Node_t* cur, NodeMap &amp;map){
    if(cur == NULL) return NULL;
    NodeMap::iterator it = map.find(cur);
    if(it != map.end())
       return *it;
    Node_t* node = new Node_t;
    map.insert(node);
    node-&gt;ptr1 = copy_recursive(cur-&gt;ptr1, map);
    node-&gt;ptr2 = copy_recursive(cur-&gt;ptr2, map);
    return node;
} 

Node_t* copy_node(Node_t* root){
     NodeMap map;
     return copy_recursive(root, map);
}</pre>
**13.9  Write a smart pointer (smart_ptr) class.  **

Check Effective C++ rule 14.
<pre>template &lt;class T&gt;
class smart_ptr {
public:
	smart_ptr(T *ptr){
		//cout&lt;&lt;"default"&lt;&lt;endl;
		ref = ptr;
		ref_count = new unsigned int();
		*ref_count = 1;
	}

	smart_ptr(smart_ptr &amp;sptr){
		//cout&lt;&lt;"copy assign"&lt;&lt;endl;
		ref = sptr.ref;
		ref_count = sptr.ref_count;
		++*ref_count;
	}

	smart_ptr&amp; operator=(smart_ptr &amp;sptr){
		//cout&lt;&lt;"= assign"&lt;&lt;endl;
		if(this != &amp;sptr &amp;&amp; ref != sptr.ref) {
			if (--*ref_count == 0){ // this may already got a ptr
				clear();
				//cout&lt;&lt;"= clear"&lt;&lt;endl;
			}

			ref = sptr.ref;
			ref_count = sptr.ref_count;
			++*ref_count;
		}
		return *this;
	}

        T operator*() { return *ref; }

	~smart_ptr(){
		//cout&lt;&lt;*ref_count&lt;&lt;endl;
		if(--*ref_count == 0){
			clear();
			//cout&lt;&lt;"clear"&lt;&lt;endl;
		}
	}
private:
	void clear(){
		delete ref;
		delete ref_count;
		ref = NULL;
		ref_count = NULL;
	}
protected:
	T *ref;
	unsigned int *ref_count;
};

int main(int argc, char const *argv[])
{
	int *p1 = new int();
	*p1 = 100;
	smart_ptr sp1(p1), sp2(p1);
	smart_ptr spa = sp1; // copy assign not = assign
	sp2 = sp1; // = assign
	cout&lt;&lt;*spa&lt;&lt;endl; // spa is the same of sp1
	return 0;
}</pre>
**15.1 Write a method to find the number of employees in each department**
<pre>SELECT Departments.name, count(Employees.id) FROM Departments, Employees WHERE Employees.depid = Departments.id(+) GROUP BY Departments.id

SELECT Departments.name, count(Employees.id) FROM Departments LEFT JOIN Employees ON Employees.depid = Departments.id GROUP BY Departments.id</pre>
**15.2 What are the different types of joins? Please explain how they differ and why certain types are better in certain situations.**

** ** There are innerjoin, left outer join, right outer join, full outer join and cross join. INNER JOIN result set will only contain datas where the criteria match.
<pre>SELECT * FROM Employees, Departments WHERE Employees.dpid = Departments.id;</pre>
CROSS JOIN returns the Cartesian product of rows from tables in the join. It will produce rows which combine each row from the first table with each row from the second table.
<pre>SELECT * FROM Employees, Departments;</pre>
LEFT OUTER JOIN returns all the values from an inner join plus all values in the left table that do not match to the right table(NULL in the case of no matching join predicate).
<pre>SELECT * FROM Employees, Departments WHERE Employees.dpid = Departments.id(+);</pre>
RIGHT OUTER JOIN returns all the values from the right table and matched values from the left table (NULL in the case of no matching join predicate). Right and left outer joins are functionally equivalent. FULL OUTER JOIN combines the effect of applying both left and right outer joins. Some database systems do not support the full outer join functionality directly.
<pre>SELECT * FROM Employees INNER JOIN Departments ON Employees.dpid = Departments.id UNION ALL;</pre>
**15.3 What is denormalization? Explain the pros and cons.**

denormalization is the process of attempting to optimize the read performance of a database by adding redundant data or by grouping data, like storing the count of the "many" objects in a one-to-many relationship as an attribute of the "one" relation or adding attributes to a relation from another relation with which it will be joined. It's do good in performance yet more redundant data.

**15.5 Imagine a simple database storing information for students’ grades. Design what this database might look like, and provide a SQL query to return a list of the honor roll students (top 10%), sorted by their grade point average.**
<pre>CREATE TABLE Students(
    'sid' INT NOT NULL AUTO_INCREMENT,
    'name' varchar(20) NOT NULL,
    'sex' enum(f,m),
    PRIMARY KEY('sid')
);

CREATE TABLE Courses(
    'cid' INT NOT NULL AUTO_INCREMENT,
    'name' varchar(20) NOT NULL,
    'description' text,
    PRIMARY KEY('cid')
);

CREATE TABLE Enrollment(
     'eid' INT NOT NULL AUTO_INCREMENT,
     'cid' INT NOT NULL,
     'sid' INT NOT NULL,
     'grade' INT NOT NULL,
     PRIMARY KEY('eid')
);

SELECT Students.name, GPA 
FROM (
     SELECT TOP 10 percent Avg(Enrollment.grade) AS GPA, Enrollment.sid 
     FROM Enrollment 
     GROUP BY Enrollment.sid 
     ORDER BY Avg(Enrollment.grade)
) Honors, Students 
WHERE Students.sid = Honors.sid;</pre>
**16.1 Explain the following terms: virtual memory, page fault, thrashing.**

_Virtual memory_ maps memory addresses used by programs to physical addresses in memory hardware, thus Main storage as seen by a process or task appears as a contiguous address space.

_Page fault_ is a trap raised by the hardware when a program accesses a page that is mapped in the virtual address space, but not loaded in physical memory.

_Thrashing_ occurs when a computer's virtual memory subsystem is in a constant state of paging, rapidly exchanging data in memory for data on disk, to the exclusion of most application-level processing, which causes the performance of the computer to degrade or collapse.

**16.2 What is a Branch Target buffer? Explain how it can be used in reducing bubble cycles in cases of branch misprediction**

The _branch target buffer_ is a true cache, the full PC value must be conpared to validate that this is a branch instruction before taking any action which reduces the penalty by predicting the path of the branch, computing the target of the branch and caching the information used by the branch. There will be no stalls if the branch entry found on BTB and the prediction is correct, otherwise the penalty will be at least two cycles.

**16.3 Describe direct memory access (DMA). Can a user level buffer/pointer be used by kernel or drivers? **

_DMA_ is a feature that allows certain hardware subsystems within the computer to access system memory independently of the CPU. By using DMA, drivers can access the memory allocated to the user level buffer/pointer.

**16.5 Write a program to find whether a machine is big endian or little endian.**
<pre>bool check_little_endian()
{
   union{
       int a;
       char b;
   }c;
   c.a = 1;
   return c.b ? true : false;
}

bool check_little_endian()
{
    short int a = 0x0001;
    char* b = (char*)&amp;a;
    return b[0] ? true : false;
}</pre>
**16.6 Discuss how would you make sure that a process doesn’t access an unauthorized part of the stack **

For native code there is the possiblility of stack overflow and remember in a multi-threaded environment, there can be multiple stacks in a process and nothing can fully prevent process access memory addresses. We can build a sandbox for the program and vm is a good example.

**16.8 A device boots with an empty FIFO queue. In the first 400ns period after startup, and in each subsequent 400ns period, a maximum of 80 words will be written to the queue. Each write takes 4ns. A worker thread requires 3ns to read a word, and 2ns to process it before reading the next word. What is the shortest depth of the FIFO such that no data is lost?**

&nbsp;

**16.9 Write an aligned malloc &amp; free function that takes number of bytes and aligned byte (which is always power of 2) EXAMPLE align_malloc(1000,128) will return a memory address that is a multiple of 128 and that points to memory of size 1000 bytes aligned_free() will free memory allocated by align_malloc **
<pre>void* align_malloc(size_t bytes, size_t aligned){
	void* p1; // original block
	void** p2; // aligend block
	// extra space for storing p1 and alignment
	int offset = aligend - 1 + sizeof(void*); 
	if ((p1 = (void*)malloc(bytes + offset)) == NULL)
		return NULL;
	// make sure p2 minus the extra space address and the result is aligned
	p2 = (void**)(((size_t)(p1) + offset) &amp; ~(aligend - 1)); 
	p2[-1] = p1; //store p1 for free operation
	return p2;
}

void align_free(void* p){
	free(((void**)p)[-1]);
}</pre>
**16.10 Write a function called my2DAlloc which allocates a two dimensional array. Minimize the number of calls to malloc and make sure that the memory is accessible by the notation arr[i][j] **
<pre>int** my2DAlloc(int r, int c){
    int header = r * sizeof(int*);
    int data = r * c * sizeof(int);
    int** arr = (int**)malloc(header + data);
    int* buf = (int*)(arr + r); // save space for r ptrs
    for(int i =0; i&lt; r; ++i)
        arr[i] = buf + i * c; //every ptr points to the right place which works for arr[i][j]
    return arr;
}</pre>
**17.1 Explain what happens, step by step, after you type a URL into a browser. Use as much detail as possible.**

In an extremely rough and simplified sketch, assuming the simplest possible HTTP request, no proxies and IPv4 (this would work similarly for IPv6-only client):

1.  browser checks cache; if requested object is in cache and is fresh, skip to #9
2.  browser asks OS for server's IP address
3.  OS makes a DNS lookup and replies the IP address to the browser
4.  browser opens a TCP connection to server (this step is much more complex with HTTPS)
5.  browser sends the HTTP request through TCP connection
6.  browser receives HTTP response and may close the TCP connection, or reuse it for another request
7.  browser checks if the response is a redirect (3xx result status codes), authorization request (401), error (4xx and 5xx), etc.; these are handled differently from normal responses (2xx)
8.  if cacheable, response is stored in cache
9.  browser decodes response (e.g. if it's gzipped)
10.  browser determines what to do with response (e.g. is it a HTML page, is it an image, is it a sound clip?)
11.  browser renders response, or offers a download dialog for unrecognized types
Again, discussion of each of these points have filled countless pages; take this as a starting point. Also, there are many other things happening in parallel to this (processing typed-in address, adding page to browser history, displaying progress to user, notifying plugins and extensions, rendering the page while it's downloading, pipelining, connection tracking for keep-alive, etc.).

**17.5 What are the differences between TCP and UDP?**

TCP is a connection-oriented protocol and it's reliable, ordered and heavyweight. UDP is connectionless protocol and it's unreliable, not ordered and lightweight.

**18.1 What’s the difference between a thread and a process?**

A process can be thought of as an instance of a program in execution. Each process is an independent entity to which system resources (CPU time, memory, etc ) are allocated and each process is executed in a separate address space.

A thread uses the same heap space of a process. A process can have multiple threads. Each thread still has its own registers and its own stack, but other threads can read and write the heap memory.

**18.2 How can you measure the time spent in a context switch?**

If the total timeof execution of all the processes was T, then the context switch time = T – (SUM for all processes (waiting time + execution time)). Overall, we can say that this is mostly an approximate calculation which depends on the underlying OS.

**18.3 Implement a singleton design pattern as a template such that, for any given class Foo, you can call Singleton::instance() and get a pointer to an instance of a singleton of type Foo. Assume the existence of a class Lock which has acquire() and release() methods How could you make your implementation thread safe and exception safe?**
<pre>class Lock {
public:
	Lock();
	~Lock();
	void AcquireLock(){};
	void RelaseLock(){};
};

template  class Singleton{
public:
	Singleton();
	~Singleton(){
		if (object != 0)
			delete object;
	}
	static T* instance();
private:
	static Lock lock;
	static T* object;
};

template 
Lock Singleton::lock;

template 
T* Singleton::object;

template 
T* Singleton::instance(){
	if(object == 0){
		lock.AcquireLock();
		if (object == 0) {
			object = new T;
		}
		lock.RelaseLock();
	}
	return object;
}

int main(int argc, char const *argv[])
{
	int* stest = Singleton::instance();
	return 0;
}</pre>
**18.5 i) Can you design a mechanism to make sure that B is executed after A, and C is ex -
ecuted after B?
ii) Suppose we have the following code to use class Foo We do not know how the threads will be scheduled in the OS
Foo f;
f.A(.....); f.B(.....); f.C(.....);
f.A(.....); f.B(.....); f.C(.....);
Can you design a mechanism to make sure that all the methods will be executed in sequence?**

i)
<pre>Semaphore a(0),b(0);
A {
   ...
   a.Release(1);
}
B {
   a.Acquire(1);
   ....
   b.Release(1);
}
C {
   b.Acquire(1);
   ...
}</pre>
ii)
<pre>Semaphore a(0),b(0),c(1);
A {
   c.Acquire(1);
   ...
   a.Release(1);
}
B {
   a.Acquire(1);
   ....
   b.Release(1);
}
C {
   b.Acquire(1);
   ...
   c.Release(1);
}</pre>