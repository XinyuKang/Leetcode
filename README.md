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
