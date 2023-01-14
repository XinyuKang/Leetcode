Some Leetcode Notes

## Categories

### DFS/BFS:

### DP:

### Recursion:

### Sliding Window:

### Backtracking:
[Backtracing Algorithm Explanation](https://en.wikipedia.org/wiki/Backtracking)

### Others:



## Python Tricks

### Create the Main Function Inside Script:
```
if __name__ == "__main__":
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
