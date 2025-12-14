# Python DSA Cheat Sheet

## Data Structures

### List

```python
# Creating and modifying a list
lst = [1, 2, 3]
lst = [0] * 5                # Create list with repeated elements: [0, 0, 0, 0, 0]
lst.append(4)                # Add to end
x = lst.pop()                # Remove and return last element
x = lst.pop(0)               # Remove and return first element
lst.insert(0, 0)             # Insert at index
lst.extend([5, 6])           # Extend with another list

# Access and slicing
first_element = lst[0]
last_element = lst[-1]
sub_list = lst[1:3]          # Elements at indices 1 and 2
rev_list = lst[::-1]         # Reverse a list

# Common operations
lst.sort()                   # Sort in ascending order # In place sorting
lst.sort(reverse=True)       # Sort in descending order
sorted_lst = sorted(lst)     # Return a new sorted list # Creates a new sorted array
length = len(lst)            # Get number of items
count = lst.count(2)         # Count occurrences of 2
index = lst.index(3)         # Find first index of 3

# Remove duplicates while preserving order
seen = set()
unique_lst = [x for x in lst if x not in seen and not seen.add(x)]
```

### Set

```python
s = set()
s.add(x)         # Add element
s.remove(x)      # Remove element
exists = x in s  # Check membership
```

### Dictionary (Hash Map)

```python
d = {'key': value}           # Create with key-value pairs
d = {}                        # Empty dictionary
d[key] = value                # Add or update
value = d.get(key, default)   # Access with default if key missing
del d[key]                    # Remove key-value pair
keys = list(d.keys())
values = list(d.values())

# Frequency Counter Pattern
freq = {}
for num in nums:
    freq[num] = freq.get(num, 0) + 1

# Using Counter from collections
from collections import Counter
nums = [1, 2, 2, 3, 3, 3]
count = Counter(nums)         # Counter({3: 3, 2: 2, 1: 1})
```

### Heap (Min-Heap / Priority Queue)

```python
import heapq

h = []
heapq.heappush(h, x)         # Push item onto heap (O(log n))
x = heapq.heappop(h)         # Pop smallest item (O(log n))
heapq.heapify(lst)           # Transform list into a heap (O(n))
min_item = h[0]              # Peek at smallest element (O(1))

# For Max-Heap using negation
heapq.heappush(h, -item)
max_item = -heapq.heappop(h)

#read about this -
top3 = heapq.nlargest(3, best.values())

```

### Deque (Double-Ended Queue)

```python
from collections import deque

q = deque()
q.append(x)                  # Enqueue at right
x = q.popleft()              # Dequeue from left
x = q.pop()                  # Dequeue from right
q.appendleft(x)              # Enqueue at left
```

### Linked List Node

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

# Example: creating a linked list with two nodes
node1 = ListNode(1)
node2 = ListNode(2)
node1.next = node2
```

## Algorithms

### Breadth-First Search (BFS)

```python
from collections import deque

def bfs(start, graph):
    q = deque([start])
    visited = set([start])
    while q:
        node = q.popleft()
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                q.append(neighbor)
```

### Depth-First Search (DFS)

```python
def dfs(node, graph, visited):
    visited.add(node)
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs(neighbor, graph, visited)
```

### Sorting

```python
# Built-in sort (Timsort, O(n log n))
lst.sort()                   # In-place ascending
lst.sort(reverse=True)       # In-place descending
sorted_lst = sorted(lst)     # New sorted list

# Example: using sort with a key
lst = ['apple', 'banana', 'cherry']
lst.sort(key=len)            # Sort by string length
```

```python
# Suppose we have a list of words:
words = ['apple', 'banana', 'cherry', 'date']

# We want to sort them by their last letter.
# The lambda takes each string s and returns s[-1]:
words.sort(key=lambda s: s[-1])

print(words)
# Output: ['banana', 'apple', 'date', 'cherry']
# Because:
#   'banana' ends with 'a'
#   'apple' ends with 'e'
#   'date'   ends with 'e'
#   'cherry' ends with 'y'
# Alphabetically: 'a' < 'e' < 'y', and ties stay in original order.

nums = [70, 1727, 507, 777, 123]
nums.sort(key=lambda n: str(n).count('7'))
print(nums)
# Output: [123, 70, 507, 1727, 777]
# Because:
#   '123'  has 0 sevens
#   '70'   has 1 seven
#   '507'  has 1 seven
#   '1727' has 2 sevens
#   '777'  has 3 sevens

LAMBDA is like ..for eveery s return s[-1] #menaing last char AMAZING

```

## Common Patterns

### List Comprehensions

```python
# Creating a list of squares
squares = [x * x for x in lst]

# Filtering elements
filtered = [x for x in lst if x > 0]

# Conditional expression inside comprehension
conditional = [x if x > 0 else 0 for x in lst]
```

### Enumerate and Zip

```python
for index, value in enumerate(lst):
    print(index, value)

for x, y in zip(lst1, lst2):
    print(x, y)
```

### Check Even or Odd Using Bitwise

```python
if n & 1:    # If result is 1, n is odd; if 0, n is even
    # Odd
else:
    # Even

# Bit shifts
half = n >> 1     # Divide by 2
double = n << 1   # Multiply by 2
```

### Common Utilities

```python
total = sum(lst)         # Sum of elements
minimum = min(lst)       # Smallest element
maximum = max(lst)       # Largest element
```

### Two Pointers

```python
left, right = 0, len(nums) - 1
while left < right:
    # Process elements nums[left] and nums[right]
    left += 1
    right -= 1
```

### Sliding Window

```python
window = {}
left = curr_sum = 0
for right in range(len(nums)):
    # Add nums[right] to window or sum
    # Process window constraints
    while condition_not_met:
        # Remove nums[left] from window or sum
        left += 1
```

### Binary Search

```python
left, right = 0, len(nums) - 1
while left <= right:
    mid = left + (right - left) // 2  # Prevents overflow
    if nums[mid] == target:
        return mid
    elif nums[mid] < target:
        left = mid + 1
    else:
        right = mid - 1
```

## Useful Tricks

### String Operations

```python
s = "hello world"
s = s.strip()             # Remove whitespace
s = s.lower()             # Convert to lowercase
joined = ''.join(lst)     # Join list elements into string
rev = s[::-1]             # Reverse string
ascii_val = ord('a')      # Get ASCII value of character
```

### Bit Operations

```python
# Check odd or even
n & 1                    # 1 if odd, 0 if even

# Shifts
half = n >> 1            # Divide by 2
double = n << 1          # Multiply by 2
```

## Tips & Best Practices

- Use `Counter` for frequency counting.
- Prefer `deque` over `list` for queue operations.
- Binary search: use `mid = left + (right - left) // 2` to prevent overflow.
- Sorting complexity: O(n log n).
- Hash map/set operations: O(1) average time.
- Heap push/pop: O(log n).
- Sliding window and two pointers techniques: O(n).

# XOR

- when both are different 1 and if same then 0
- 2 ^ 3 ===> '01' ^ '11' ===> '01'
- 2 ^ 2 ===> '01' ^ '01' (all bit same) ==> hence zero
- 0 ^ 2 ===> answer is '01' (2)

## Time Complexities (Worst Case) for Common DSA Operations

### List / Array

- **Access by index** (`lst[i]`): O(1)
- **Append at end** (`lst.append(x)`): O(1) amortized (worst-case O(n) when resizing)
- **Pop from end** (`lst.pop()`): O(1)
- **Insert / Delete at arbitrary index** (`lst.insert(i, x)` or `del lst[i]`): O(n)
- **Search (linear)** (`x in lst`): O(n)

### Dictionary (Hash Map)

- **Lookup / Membership** (`d[key]`, `key in d`): O(n)
- **Insert / Update** (`d[key] = value`): O(n)
- **Delete** (`del d[key]`): O(n)

_(Average-case is O(1); worst-case due to hash collisions is O(n).)_

### Set (Hash Set)

- **Insert** (`s.add(x)`): O(n)
- **Lookup / Membership** (`x in s`): O(n)
- **Delete** (`s.remove(x)` / `s.discard(x)`): O(n)

_(Average-case is O(1); worst-case due to hash collisions is O(n).)_

### Deque (`collections.deque`)

- **Append / Appendleft** (`q.append(x)`, `q.appendleft(x)`): O(1)
- **Pop / Popleft** (`q.pop()`, `q.popleft()`): O(1)
- **Indexing by position** (`q[i]`): O(n)

### Heap (`heapq`)

- **Push** (`heapq.heappush(heap, x)`): O(log n)
- **Pop (remove min)** (`heapq.heappop(heap)`): O(log n)
- **Heapify** (`heapq.heapify(lst)`): O(n)
- **Peek min** (`heap[0]`): O(1)

### Sorting

- **Built-in sort** (`lst.sort()` / `sorted(lst)`): O(n log n)

### Search Patterns

- **Binary Search (on sorted array)**: O(log n)
- **BFS / DFS Traversal (on graph)**: O(V + E)  
  _(V = number of vertices, E = number of edges)_

### Common Utility Functions

- **Length** (`len(obj)`): O(1)
- **Min / Max** (`min(obj)` / `max(obj)`): O(n)
- **Sum** (`sum(lst)`): O(n)
- **All / Any** (`all(iterable)` / `any(iterable)`): O(n)
- **Zip** (`zip(lst1, lst2)`): O(min(n₁, n₂))
