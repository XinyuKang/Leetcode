Some Leetcode Notes

## Categories

### DFS/BFS:

### DP:

### Recursion:

### Sliding Window:

### Backtracking:

### Others:



## Python Tricks

### Dictionary: 
Unordered key-value pair. The functionality of both dictionaries and defaultdict are almost same except for the fact that defaultdict never raises a KeyError. It provides a default value for the key that does not exists (each key has the same default value). e.g. defaultdict(int) returns a dictionary with all default value 0.

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
    
### Mutate a string in Python:
String type is immutable in Python

(1) Replace non-alphabetic characters in the string: 
    ```
    import re
    s = re.sub(r'^[a-zA-Z0-9]', "", s)
    ```

(2) using the "replace" function to replace "counts" of a character to another character:
    ```
    some_str.replace("a", "b", counts)
    ```
(3) replace a character at a certain index:
    ```
    s = s[:index] + newstring + s[index + 1:]
    ```

