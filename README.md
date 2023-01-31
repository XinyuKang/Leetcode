Some Leetcode Notes

## Categories

### BFS:
问题的本质就是让你在一幅「图」中找到从起点 start 到终点 target 的最近距离

形象点说，DFS 是线，BFS 是面；DFS 是单打独斗，BFS 是集体行动

BFS 可以找到最短距离，但是空间复杂度高，而 DFS 的空间复杂度较低

由此观之，BFS 还是有代价的，一般来说在找最短路径的时候使用 BFS，其他时候还是 DFS 使用得多一些

大致思路：
```
// 计算从起点 start 到终点 target 的最近距离
int BFS(Node start, Node target) {
    Queue<Node> q; // 核心数据结构
    Set<Node> visited; // 避免走回头路
    
    q.offer(start); // 将起点加入队列
    visited.add(start);
    int step = 0; // 记录扩散的步数

    while (q not empty) {
        int sz = q.size();
        /* 将当前队列中的所有节点向四周扩散 */
        for (int i = 0; i < sz; i++) {
            Node cur = q.poll();
            /* 划重点：这里判断是否到达终点 */
            if (cur is target)
                return step;
            /* 将 cur 的相邻节点加入队列 */
            for (Node x : cur.adj()) {
                if (x not in visited) {
                    q.offer(x);
                    visited.add(x);
                }
            }
        }
        /* 划重点：更新步数在这里 */
        step++;
    }
}
```
#### 双向BFS
传统的 BFS 框架就是从起点开始向四周扩散，遇到终点时停止；而双向 BFS 则是从起点和终点同时开始扩散，当两边有交集的时候停止

双向 BFS 是要比传统 BFS 高效的：
![double directed BFS vs BFS](https://github.com/XinyuKang/Leetcode/blob/main/double-bfs.jpg)

不过，双向 BFS 也有局限，因为你必须知道终点在哪里

大致思路（以Leetcode 752为例）：
```
int openLock(String[] deadends, String target) {
    Set<String> deads = new HashSet<>();
    for (String s : deadends) deads.add(s);
    // 用集合不用队列，可以快速判断元素是否存在
    Set<String> q1 = new HashSet<>();
    Set<String> q2 = new HashSet<>();
    Set<String> visited = new HashSet<>();
    
    int step = 0;
    q1.add("0000");
    q2.add(target);
    
    while (!q1.isEmpty() && !q2.isEmpty()) {
        // 哈希集合在遍历的过程中不能修改，用 temp 存储扩散结果
        Set<String> temp = new HashSet<>();

        /* 将 q1 中的所有节点向周围扩散 */
        for (String cur : q1) {
            /* 判断是否到达终点 */
            if (deads.contains(cur))
                continue;
            if (q2.contains(cur))
                return step;
            
            visited.add(cur);

            /* 将一个节点的未遍历相邻节点加入集合 */
            for (int j = 0; j < 4; j++) {
                String up = plusOne(cur, j);
                if (!visited.contains(up))
                    temp.add(up);
                String down = minusOne(cur, j);
                if (!visited.contains(down))
                    temp.add(down);
            }
        }
        /* 在这里增加步数 */
        step++;
        // temp 相当于 q1
        // 这里交换 q1 q2，下一轮 while 就是扩散 q2
        q1 = q2;
        q2 = temp;
    }
    return -1;
}
```
### Binary Tree
二叉树的所有问题，就是让你在前中后序位置注入巧妙的代码逻辑，去达到自己的目的，你只需要单独思考每一个节点应该做什么，其他的不用你管，抛给二叉树遍历框架，递归会在所有节点上做相同的操作。

前序位置的代码只能从函数参数中获取父节点传递来的数据，而后序位置的代码不仅可以获取参数数据，还可以获取到子树通过函数返回值传递回来的数据。

举具体的例子，现在给你一棵二叉树，我问你两个简单的问题：

1、如果把根节点看做第 1 层，如何打印出每一个节点所在的层数？

2、如何打印出每个节点的左右子树各有多少节点？

这两个问题的根本区别在于：一个节点在第几层，你从根节点遍历过来的过程就能顺带记录；而以一个节点为根的整棵子树有多少个节点，你需要遍历完子树之后才能数清楚。

只有后序位置才能通过返回值获取子树的信息，所以有时运行速度会更短， 所以遇到子树问题，首先想到的是给函数设置返回值，然后在后序位置做文章，如果你写出了类似一开始的那种递归套递归的解法，大概率也需要反思是不是可以通过后序遍历优化了。

通过前序中序，或者后序中序遍历结果可以确定唯一一棵原始二叉树，但是通过前序后序遍历结果无法确定唯一的原始二叉树。

### DP:

### Recursion:

### Sliding Window:
最难的一类双指针问题：维护一个窗口，不断滑动，然后更新答案，Time Complexity = O(n)
大致思路：
```
/* 滑动窗口算法框架 */
void slidingWindow(string s) {
    unordered_map<char, int> window;
    
    int left = 0, right = 0;
    while (right < s.size()) {
        // c 是将移入窗口的字符
        char c = s[right];
        // 增大窗口
        right++;
        // 进行窗口内数据的一系列更新
        ...

        /*** debug 输出的位置 ***/
        // 注意在最终的解法代码中不要 print
        // 因为 IO 操作很耗时，可能导致超时
        printf("window: [%d, %d)\n", left, right);
        /********************/
        
        // 判断左侧窗口是否要收缩
        while (window needs shrink) {
            // d 是将移出窗口的字符
            char d = s[left];
            // 缩小窗口
            left++;
            // 进行窗口内数据的一系列更新
            ...
        }
    }
}

# 这两个 ... 处的操作分别是扩大和缩小窗口的更新操作，它们操作是完全对称的
# left = right = 0，把索引左闭右开区间 [left, right) 称为一个「窗口」
# 明显的滑动窗口算法: 给你一个 S 和一个 T，请问你 S 中是否存在一个子串，包含 T 中所有字符且不包含其他字符
```

### Backtracking:
[Backtracing Algorithm Explanation](https://en.wikipedia.org/wiki/Backtracking)

对于递归相关的算法，时间复杂度这样计算（递归次数）*（递归函数本身的时间复杂度）

大致思路：
```
result = []
def backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)
        return
    
    for 选择 in 选择列表:
        做选择
        backtrack(路径, 选择列表)
        撤销选择
# 回溯算法和 DFS 算法的细微差别是：回溯算法是在遍历「树枝」，DFS 算法是在遍历「节点」
```
回溯算法秒杀所有排列-组合-子集问题:

形式一、元素无重不可复选，即 nums 中的元素都是唯一的，每个元素最多只能被使用一次，这也是最基本的形式。

形式二、元素可重不可复选，即 nums 中的元素可以存在重复，每个元素最多只能被使用一次。

形式三、元素无重可复选，即 nums 中的元素都是唯一的，每个元素可以被使用若干次。

上面用组合问题举的例子，但排列、组合、子集问题都可以有这三种基本形式，所以共有 9 种变化。

记住这两种树形结构就能解决所有相关问题:

![backtracking tree 1](https://github.com/XinyuKang/Leetcode/blob/main/backtracking1.jpg)
![backtracking tree 2](https://github.com/XinyuKang/Leetcode/blob/main/backtracking2.jpg)
### Task Scheduling:
问题：n项任务，有加工时间t1,t2,...tn, 只有一台机器

目标：所有task的总用时之和最短

解法：按用时从小到大排


问题：n项任务，有加工时间t1,t2,...tn, 有截止时间d1,d2,...dn, 同样只有一台机器

目标：单项任务最大延迟达到最小（最大延迟定义为超出截止时间的长度）

解法：按截止时间从前到后排


问题：n项任务，有开始时间结束时间和overlaps，只有一台机器

目标：完成最多的tasks

解法：按结束时间从前到后加入被选集


问题：n项任务，有开始时间结束时间和overlaps，有多个processors

目标：使用最少数量的processors完成所有任务

解法：按开始时间从前到后加入processor中，若有冲突，create a new processor



### Others:



## Python Tricks

### Create the Main Function Inside Script:
```
if __name__ == "__main__":
```

### Binary Int
```
# int(value, base): the base param here indicated what is the base of the given value, the int() itself returns the sum value of base 10
# bin(36): return the binary version of 36

a = "1101"
b = "100"
 
# Calculating binary value using function
sum = bin(int(a, 2) + int(b, 2))   
 
# Printing result
print(sum[2:])  # this is because bin(num) will return "0bxxx" and [2:] is the real num
```
### List

#### Append to list at a certain index:
```
# list.insert() – inserts a single element anywhere in the list. insert(index, element)
odd_n.insert(3, 21)

# list.append() – always adds items (strings, numbers, lists) at the end of the list.
crops.append('cane')

# list.extend() – adds iterable items (lists, tuples, strings) to the end of the list.
even_numbers = [2, 4, 8]
more_even_numers = [100, 400]
even_numbers.extend(more_even_numers)

# list.pop(idx): if idx is not given, the last element will be popped
```
#### Find the index of a number in a list:
```
# list of items
list2 = ['cat', 'bat', 'mat', 'cat', 'pet']
 
# Will print the index of 'bat' in list2
print(list2.index('bat'))
```

#### list + list is a new list
```
thislist = ["apple", "banana", "cherry"]
print(thislist + ["hello"])    # ['apple', 'banana', 'cherry', 'hello']
```

#### Flatten a list using sum()
sum(iterable, start)  

iterable : iterable can be anything list , tuples or dictionaries ,
 but most importantly it should be numbers.
 
start : this start is added to the sum of 
numbers in the iterable. 
If start is not given in the syntax , it is assumed to be 0.

```
ini_list = [[1, 2, 3],
            [3, 6, 7],
            [7, 5, 4]]
 
# printing initial list
print ("initial list ", str(ini_list))
 
# converting 2d list into 1d
flatten_list = sum(ini_list, [])
 
# printing flatten_list
print ("final_result", str(flatten_list))  # [1, 2, 3, 3, 6, 7, 7, 5, 4]
```

### Loop two variables simultaneously  in a for loop
```
for i, j in zip(range(x), range(y)):
```
### Deque
Deque (Doubly Ended Queue) in Python is implemented using the module “collections“. Deque is preferred over a list in the cases where we need quicker append and pop operations from both the ends of the container, as deque provides an O(1) time complexity for append and pop operations as compared to a list that provides O(n) time complexity.
```
from collections import deque 
      
# Declaring deque 
de = deque(['name','age','DOB'])  
de.append(4)
de.appendleft(6)
de.pop()
de.popleft()

# using insert() to insert the value 3 at 5th position
de.insert(4,3)

# using count() to count the occurrences of 3
print ("The count of 3 in deque is : ")
print (de.count(3))

# using remove() to remove the first occurrence of 3
de.remove(3)
```

### Dictionary: 
Unordered key-value pair. The functionality of both dictionaries and defaultdict are almost same except for the fact that defaultdict never raises a KeyError. It provides a default value for the key that does not exists (each key has the same default value). e.g. defaultdict(int) returns a dictionary with all default value 0.
#### Some methods:
```
# Length of a dictionary:
len(dict)

# Get an item:
dict[key]
dict.get(key)

# Append item to a dict:
dict["appended"] = appended_val
dict.update({"appended": appended_val})

# Delete an item:
dict.pop(key)

# Delete a random item:
dict.popitem()

# Delete all items:
dict.clear()
```
#### Inverse the Mapping of a Dictionary:
```
inv_map = {v: k for k, v in my_map.items()}  # Dictionary also has Comprehension
```
#### Sort the Dictionary:
```
people = {3: "Jim", 2: "Jack", 4: "Jane", 1: "Jill"}

# Sort by key
dict(sorted(people.items()))
# {1: 'Jill', 2: 'Jack', 3: 'Jim', 4: 'Jane'}

# Sort by value
dict(sorted(people.items(), key=lambda item: item[1]))   # people.items() is a list of (key, value) pairs
# {2: 'Jack', 4: 'Jane', 1: 'Jill', 3: 'Jim'}
```

#### defaultdict()
```
from collections import defaultdict
dict = defaultdict(callable)

# the parameter must be a callable: int or self-defined function, e.g.:
def return_true:
    return True
dict = defaultdict(return_true)

```

### Set:
Set data structure is referred as Unordered Collections of Unique Elements and that doesn't support operations like indexing or slicing etc.

### Counter: 
Count several repeated objects at once. Returns a python dictionary with the counts of each letter. The .update() implementation provided by Counter adds existing counts together.

### Get Character Position in Alphabet:
0-indexed: 
```
def char_position(letter):
    return ord(letter) - 97

def pos_to_char(pos):
    return chr(pos + 97)
```
    
### Mutate a String in Python:
String type is immutable in Python

#### Replace non-alphabetic characters in the string: 
    ```
    import re
    s = re.sub(r'^[a-zA-Z0-9]', "", s)
    ```

#### Using the "replace" function to replace "counts" of a character to another character:
    ```
    some_str.replace("a", "b", counts)
    ```
#### Replace a character at a certain index:
    ```
    s = s[:index] + newstring + s[index + 1:]
    ```
#### Sort the letters in a string alphabetically
```
a = 'ZENOVW'
b = sorted(a)
# b = ['E', 'N', 'O', 'V', 'W', 'Z']
# sorted returns a list, so you can make it a string again using join:
c = ''.join(b)
```

#### Find Number of Unique Characters in String
```
# Method 1
count = len(set(str))
# Method 2
from collections import Counter
count = len(Counter(str))
```

#### join()
join() is an inbuilt string function in Python used to join elements of the sequence separated by a string separator. This function joins elements of a sequence and makes it a string. 
```
# elements in tuples
list1 = ('1', '2', '3', '4')
 
# put any character to join
s = "-"
 
# joins elements of list1 by '-'
# and stores in string s
s = s.join(list1)  # 1-2-3-4
```

#### Get the position of a character in Python using rfind()
Python String str.rfind(sub, start, end) method returns the rightmost index of the substring if found in the given string. If not found then it returns -1.
```
string = 'Geeks'
letter = 'k'
print(string.rfind(letter))   # 3
```

#### Swapping chars in string
```
def swap(s, i, j):
    lst = list(s)
    lst[i], lst[j] = lst[j], lst[i]
    return ''.join(lst)
```

#### Check if a string is palindromic:
```
# function which return reverse of a string
def isPalindrome(s):
    return s == s[::-1]
```

### Check Whether a Number is Even or Odd:
```
if (num % 2) == 0:
   print (“The number is even”)

else:
   print (“The provided number is odd”)
```

### Difference Between Break and Continue:
The break statement can be used if you need to break out of a for or while loop and move onto the next section of code. The continue statement can be used if you need to skip the current iteration of a for or while loop and move onto the next iteration.

### Sum of Digits of a Number in Python
```
sum = 0
for digit in str(n):
    sum += int(digit)      
return sum
```
